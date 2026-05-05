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

<div class="reviews-search-wrapper">
  <input
    type="search"
    class="table-search-input"
    id="reviews-search"
    placeholder="Search reviews — restaurant, bar, dish, anything…"
    aria-label="Search reviews"
    autocomplete="off">
  <span class="table-search-count" id="reviews-search-count" aria-live="polite"></span>
</div>

<div class="reviews-jumpnav" id="reviews-jumpnav">
Jump to:
{% for yr in years_seen %}<a href="#{{ yr }}" data-year="{{ yr }}">{{ yr }}</a>{% unless forloop.last %} <span class="jumpnav-sep"> · </span>{% endunless %}{% endfor %}
</div>

{% for yr in years_seen %}<section class="reviews-year" data-year="{{ yr }}">
<h2 id="{{ yr }}" class="reviews-year-heading">{{ yr }}</h2>
<ul class="reviews-list">
{% for entry in sorted %}{% assign parts = entry | split: "|" %}{% if parts[1] == yr %}<li data-post-url="{{ parts[2] }}"><a href="{{ parts[2] }}">{{ parts[3] | replace: "&#124;", "|" }}</a></li>
{% endif %}{% endfor %}</ul>
</section>
{% endfor %}

<div class="reviews-no-matches" id="reviews-no-matches" hidden>
  No reviews match that search.
</div>

{%- comment -%}
  Inline search index: each post's title + body text, used for full-text matching.
  Stripped of HTML tags and lowercased. ~88 posts × ~1500 chars = ~130KB.
  This loads on /reviews/ only, not on every page.
{%- endcomment -%}
<script id="rccc-reviews-index" type="application/json">
{
  "posts": [
    {%- for post in site.posts -%}
    {
      "url": {{ post.url | jsonify }},
      "text": {{ post.title | append: " " | append: post.content | strip_html | downcase | replace: "\n", " " | jsonify }}
    }{% unless forloop.last %},{% endunless %}
    {%- endfor -%}
  ]
}
</script>

<script>
(function () {
  var input    = document.getElementById('reviews-search');
  var counter  = document.getElementById('reviews-search-count');
  var jumpnav  = document.getElementById('reviews-jumpnav');
  var noMatch  = document.getElementById('reviews-no-matches');
  if (!input) return;

  // Build a map of url => searchable text from the inline index
  var textByUrl = {};
  try {
    var data = JSON.parse(document.getElementById('rccc-reviews-index').textContent);
    data.posts.forEach(function (p) { textByUrl[p.url] = p.text; });
  } catch (e) {}

  var allItems    = Array.prototype.slice.call(document.querySelectorAll('.reviews-list li'));
  var allSections = Array.prototype.slice.call(document.querySelectorAll('.reviews-year'));
  var allJumpLinks = Array.prototype.slice.call(jumpnav.querySelectorAll('a'));
  var jumpSeps     = Array.prototype.slice.call(jumpnav.querySelectorAll('.jumpnav-sep'));
  var totalItems = allItems.length;

  function updateCount(matched) {
    if (input.value.trim() === '') {
      counter.textContent = '';
      return;
    }
    if (matched === 0)      counter.textContent = 'No matches';
    else if (matched === 1) counter.textContent = '1 match';
    else                    counter.textContent = matched + ' matches of ' + totalItems;
  }

  function applyFilter() {
    var q = input.value.trim().toLowerCase();

    if (q === '') {
      // Reset everything
      allItems.forEach(function (li)   { li.style.display = ''; });
      allSections.forEach(function (s) { s.style.display = ''; });
      allJumpLinks.forEach(function (a){ a.style.display = ''; });
      jumpSeps.forEach(function (s)    { s.style.display = ''; });
      noMatch.hidden = true;
      updateCount(totalItems);
      return;
    }

    var terms = q.split(/\s+/);
    var matched = 0;
    var yearsWithMatch = {};

    allItems.forEach(function (li) {
      var url = li.getAttribute('data-post-url');
      var corpus = (textByUrl[url] || '') + ' ' + li.textContent.toLowerCase();
      var allMatch = terms.every(function (t) { return corpus.indexOf(t) !== -1; });
      if (allMatch) {
        li.style.display = '';
        matched++;
        // Track which year section this belongs to
        var section = li.closest('.reviews-year');
        if (section) yearsWithMatch[section.getAttribute('data-year')] = true;
      } else {
        li.style.display = 'none';
      }
    });

    // Hide year sections with no visible items
    allSections.forEach(function (s) {
      var yr = s.getAttribute('data-year');
      s.style.display = yearsWithMatch[yr] ? '' : 'none';
    });

    // Hide jumpnav years that have no matches (and their separators)
    allJumpLinks.forEach(function (a, i) {
      var yr = a.getAttribute('data-year');
      a.style.display = yearsWithMatch[yr] ? '' : 'none';
    });
    // Hide separators when neighbouring link is hidden
    jumpSeps.forEach(function (sep, i) {
      // sep[i] sits between link[i] and link[i+1]
      var leftVisible  = allJumpLinks[i]   && allJumpLinks[i].style.display   !== 'none';
      var rightVisible = allJumpLinks[i+1] && allJumpLinks[i+1].style.display !== 'none';
      sep.style.display = (leftVisible && rightVisible) ? '' : 'none';
    });

    noMatch.hidden = matched > 0;
    updateCount(matched);
  }

  input.addEventListener('input', applyFilter);
  input.addEventListener('keydown', function (e) {
    if (e.key === 'Escape') {
      input.value = '';
      applyFilter();
      input.blur();
    }
  });

  // Allow ?q=foo for shareable filtered URLs
  var params = new URLSearchParams(window.location.search);
  var initial = params.get('q');
  if (initial) {
    input.value = initial;
    applyFilter();
  }
})();
</script>
