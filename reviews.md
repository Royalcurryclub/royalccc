---
id: 1089
title: Reviews
date: '2019-09-25T14:08:20+12:00'
author: RCCC
layout: page
guid: 'http://royalccc.net/?page_id=1089'
permalink: /reviews/
---

{%- comment -%}
  Group and sort posts by their visitation month/year, parsed from the post title,
  rather than the post's publish date. This is necessary because some reviews are
  written up months after the visitation (e.g. a March visit posted in June), so
  post.date doesn't reflect the chronological order of visits.

  Title format: "March 2024 – ...", "November 2024 Annual Outing – ..."

  Strategy:
  1. For each post, extract a sortable key from the title: "YYYYMM" plus a tiebreaker.
  2. Sort all posts globally by that key, descending (newest visit first).
  3. Group them by year (also extracted from title).
{%- endcomment -%}

{%- assign month_names = "january,february,march,april,may,june,july,august,september,october,november,december" | split: "," -%}

{%- comment -%} Build sortable list of {key, year, post} {%- endcomment -%}
{%- assign sortable = "" | split: "" -%}
{%- for post in site.posts -%}
  {%- assign tlow = post.title | downcase -%}
  {%- assign found_year = "" -%}
  {%- assign found_month_num = "" -%}
  {%- for mname in month_names -%}
    {%- if tlow contains mname -%}
      {%- assign idx = forloop.index -%}
      {%- if idx < 10 -%}
        {%- assign mnum = "0" | append: idx -%}
      {%- else -%}
        {%- assign mnum = idx | append: "" -%}
      {%- endif -%}
      {%- assign rest = tlow | split: mname | last -%}
      {%- assign year_match = rest | strip | truncate: 4, "" -%}
      {%- if year_match contains "20" -%}
        {%- assign found_month_num = mnum -%}
        {%- assign found_year = year_match -%}
        {%- break -%}
      {%- endif -%}
    {%- endif -%}
  {%- endfor -%}

  {%- comment -%} Fall back to post.date if parsing failed {%- endcomment -%}
  {%- if found_year == "" -%}
    {%- assign found_year = post.date | date: "%Y" -%}
    {%- assign found_month_num = post.date | date: "%m" -%}
  {%- endif -%}

  {%- comment -%} Tiebreaker: append publish-date day so multi-event months stay deterministic {%- endcomment -%}
  {%- assign day = post.date | date: "%d" -%}
  {%- assign sort_key = found_year | append: found_month_num | append: day -%}

  {%- capture entry -%}{{ sort_key }}|{{ found_year }}|{{ post.url }}|{{ post.title | replace: "|", "&#124;" }}{%- endcapture -%}
  {%- assign sortable = sortable | push: entry -%}
{%- endfor -%}

{%- assign sorted = sortable | sort | reverse -%}

{%- comment -%} Determine the unique years in order {%- endcomment -%}
{%- assign years_seen = "" | split: "" -%}
{%- for entry in sorted -%}
  {%- assign parts = entry | split: "|" -%}
  {%- assign yr = parts[1] -%}
  {%- unless years_seen contains yr -%}
    {%- assign years_seen = years_seen | push: yr -%}
  {%- endunless -%}
{%- endfor -%}
<p>All RCCC visitations and reviews, grouped by year. Most recent first.</p>

<div class="reviews-jumpnav">
Jump to:
{% for yr in years_seen %}<a href="#{{ yr }}">{{ yr }}</a>{% unless forloop.last %} · {% endunless %}{% endfor %}
</div>

{% for yr in years_seen %}<section class="reviews-year">
<h2 id="{{ yr }}" class="reviews-year-heading">{{ yr }}</h2>
<ul class="reviews-list">
{% for entry in sorted %}{% assign parts = entry | split: "|" %}{% if parts[1] == yr %}<li><a href="{{ parts[2] }}">{{ parts[3] | replace: "&#124;", "|" }}</a></li>
{% endif %}{% endfor %}</ul>
</section>
{% endfor %}
