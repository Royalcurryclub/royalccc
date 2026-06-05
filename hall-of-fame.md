---
title: Hall of Fame
layout: page
permalink: /hall-of-fame/
---

{%- comment -%}
  ============================================================
  HALL OF FAME — auto-generated from _data/visitations.yml
  and _data/scores.yml at build time. Updates itself when new
  visits or scores are added; no manual maintenance required.
  ============================================================
{%- endcomment -%}

{%- assign visits = site.data.visitations.visitations -%}
{%- assign scores = site.data.scores.scores -%}
{%- assign visit_count = visits | size -%}
{%- assign scored_count = scores | size -%}

<p class="hof-intro">
After {{ visit_count }} sittings of curry, beer, and questionable behaviour, here is what the data tells us.
</p>

{%- comment -%} ===== CURRY ESTABLISHMENTS BY VISIT COUNT ===== {%- endcomment -%}
{%- assign curry_names = "" | split: "" -%}
{%- for v in visits -%}
  {%- if v.curry and v.curry != "---" and v.curry != "" -%}
    {%- assign curry_names = curry_names | push: v.curry -%}
  {%- endif -%}
{%- endfor -%}
{%- assign curry_grouped = curry_names | group_by: "" -%}

{%- comment -%}
  Liquid doesn't have a Counter primitive — we have to do it manually.
  Build "count|name" strings, then sort by count desc and split.
{%- endcomment -%}
{%- assign curry_unique = curry_names | uniq -%}
{%- assign curry_lines = "" | split: "" -%}
{%- for name in curry_unique -%}
  {%- assign matches = curry_names | where_exp: "n", "n == name" -%}
  {%- assign count = matches | size -%}
  {%- assign padded = count | prepend: "0000" -%}
  {%- assign padded = padded | slice: -4, 4 -%}
  {%- capture line -%}{{ padded }}|{{ count }}|{{ name }}{%- endcapture -%}
  {%- assign curry_lines = curry_lines | push: line -%}
{%- endfor -%}
{%- assign curry_sorted = curry_lines | sort | reverse -%}

{%- comment -%} Same for drinks ===== {%- endcomment -%}
{%- assign drink_names = "" | split: "" -%}
{%- for v in visits -%}
  {%- if v.drinks and v.drinks != "---" and v.drinks != "" -%}
    {%- assign drink_names = drink_names | push: v.drinks -%}
  {%- endif -%}
{%- endfor -%}
{%- assign drink_unique = drink_names | uniq -%}
{%- assign drink_lines = "" | split: "" -%}
{%- for name in drink_unique -%}
  {%- assign matches = drink_names | where_exp: "n", "n == name" -%}
  {%- assign count = matches | size -%}
  {%- assign padded = count | prepend: "0000" -%}
  {%- assign padded = padded | slice: -4, 4 -%}
  {%- capture line -%}{{ padded }}|{{ count }}|{{ name }}{%- endcapture -%}
  {%- assign drink_lines = drink_lines | push: line -%}
{%- endfor -%}
{%- assign drink_sorted = drink_lines | sort | reverse -%}

{%- comment -%} ===== HIGHEST and LOWEST scores ===== {%- endcomment -%}
{%- assign score_lines = "" | split: "" -%}
{%- for s in scores -%}
  {%- assign total_str = s.total | append: "" -%}
  {%- assign total_x10 = total_str | times: 10 | floor -%}
  {%- assign padded = total_x10 | prepend: "0000" -%}
  {%- assign padded = padded | slice: -4, 4 -%}
  {%- capture line -%}{{ padded }}|{{ s.total }}|{{ s.when }}|{{ s.visitation }}|{{ s.review_url }}{%- endcapture -%}
  {%- assign score_lines = score_lines | push: line -%}
{%- endfor -%}
{%- assign scores_sorted = score_lines | sort | reverse -%}

{%- comment -%} ===== AVERAGE BY YEAR ===== {%- endcomment -%}
{%- assign years_list = "" | split: "" -%}
{%- for s in scores -%}
  {%- assign yy = s.when | split: "-" | last | strip | slice: 0, 2 -%}
  {%- assign full_year = "20" | append: yy -%}
  {%- unless years_list contains full_year -%}
    {%- assign years_list = years_list | push: full_year -%}
  {%- endunless -%}
{%- endfor -%}
{%- assign years_sorted = years_list | sort -%}

<section class="hof-section">
  <h2>Most-visited curry establishments</h2>
  <ol class="hof-list">
    {%- for line in curry_sorted limit: 10 -%}
      {%- assign parts = line | split: "|" -%}
      {%- assign count = parts[1] -%}
      {%- assign name = parts[2] -%}
      {%- if count != "1" -%}
      <li><span class="hof-count">{{ count }}</span> visits — <span class="hof-name">{{ name }}</span></li>
      {%- endif -%}
    {%- endfor -%}
  </ol>
</section>

<section class="hof-section">
  <h2>Most-visited bars</h2>
  <ol class="hof-list">
    {%- for line in drink_sorted limit: 10 -%}
      {%- assign parts = line | split: "|" -%}
      {%- assign count = parts[1] -%}
      {%- assign name = parts[2] -%}
      {%- if count != "1" -%}
      <li><span class="hof-count">{{ count }}</span> visits — <span class="hof-name">{{ name }}</span></li>
      {%- endif -%}
    {%- endfor -%}
  </ol>
</section>

<section class="hof-section">
  <h2>Highest-scoring meals</h2>
  <ol class="hof-list">
    {%- for line in scores_sorted limit: 5 -%}
      {%- assign parts = line | split: "|" -%}
      {%- assign total = parts[1] -%}
      {%- assign when = parts[2] -%}
      {%- assign vis = parts[3] -%}
      {%- assign url = parts[4] -%}
      <li>
        <span class="hof-count hof-score-good">{{ total }}/10</span> —
        {%- if url != "" -%}
        <a href="{{ url }}" class="hof-name">{{ when }} · {{ vis }}</a>
        {%- else -%}
        <span class="hof-name">{{ when }} · {{ vis }}</span>
        {%- endif -%}
      </li>
    {%- endfor -%}
  </ol>
</section>

<section class="hof-section">
  <h2>Lowest-scoring meals</h2>
  <ol class="hof-list">
    {%- assign reversed = scores_sorted | reverse -%}
    {%- for line in reversed limit: 5 -%}
      {%- assign parts = line | split: "|" -%}
      {%- assign total = parts[1] -%}
      {%- assign when = parts[2] -%}
      {%- assign vis = parts[3] -%}
      {%- assign url = parts[4] -%}
      <li>
        <span class="hof-count hof-score-bad">{{ total }}/10</span> —
        {%- if url != "" -%}
        <a href="{{ url }}" class="hof-name">{{ when }} · {{ vis }}</a>
        {%- else -%}
        <span class="hof-name">{{ when }} · {{ vis }}</span>
        {%- endif -%}
      </li>
    {%- endfor -%}
  </ol>
</section>

<section class="hof-section hof-jalfrezi">
  <h2>The Jalfrezi Index</h2>

  {%- comment -%}
    Count posts mentioning Jalfrezi in any spelling.
    Using "jalfr" as the substring captures every plausible variant
    (Jalfrezi, Jalfrezee, Jalfrazi, Jhalfrezi, etc.) — no English word
    other than Jalfrezi-style names starts with these five letters.
    Match is case-insensitive via downcasing the content first.
  {%- endcomment -%}
  {%- assign jalfrezi_posts = 0 -%}
  {%- assign total_posts = site.posts | size -%}
  {%- for post in site.posts -%}
    {%- assign lower = post.content | downcase -%}
    {%- if lower contains "jalfr" -%}
      {%- assign jalfrezi_posts = jalfrezi_posts | plus: 1 -%}
    {%- endif -%}
  {%- endfor -%}

  <div class="jalfrezi-stat">
    <span class="jalfrezi-number">{{ jalfrezi_posts }}</span>
    <span class="jalfrezi-of">of {{ total_posts }} reviews</span>
  </div>

  <p class="jalfrezi-note">
    The noble Jalfrezi has been ordered in <strong>{{ jalfrezi_posts }}</strong>
    of the {{ total_posts }} written reviews &mdash; a number that, by all reasonable accounts,
    will only continue to climb. The responsible party is well known to this club but shall not be named here.
  </p>
</section>

<section class="hof-section">
  <h2>Average score by year</h2>
  <table class="hof-year-table">
    <thead>
      <tr><th>Year</th><th>Visits</th><th>Average</th></tr>
    </thead>
    <tbody>
    {%- for year in years_sorted -%}
      {%- assign year_short = year | slice: 2, 2 -%}
      {%- assign year_total = 0 -%}
      {%- assign year_count = 0 -%}
      {%- for s in scores -%}
        {%- assign s_yy = s.when | split: "-" | last | strip | slice: 0, 2 -%}
        {%- if s_yy == year_short -%}
          {%- assign total_num = s.total | times: 1 -%}
          {%- assign year_total = year_total | plus: total_num -%}
          {%- assign year_count = year_count | plus: 1 -%}
        {%- endif -%}
      {%- endfor -%}
      {%- if year_count > 0 -%}
        {%- assign avg = year_total | divided_by: year_count -%}
        <tr>
          <td>{{ year }}</td>
          <td>{{ year_count }}</td>
          <td>{{ avg | round: 2 }}</td>
        </tr>
      {%- endif -%}
    {%- endfor -%}
    </tbody>
  </table>
</section>

<p class="hof-footer">
Out of {{ visit_count }} sittings, {{ scored_count }} have been formally scored.
</p>
