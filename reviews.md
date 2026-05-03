---
id: 1089
title: Reviews
date: '2019-09-25T14:08:20+12:00'
author: RCCC
layout: page
guid: 'http://royalccc.net/?page_id=1089'
permalink: /reviews/
---

{%- assign posts_by_year = site.posts | group_by_exp: "post", "post.date | date: '%Y'" -%}
<p>All RCCC visitations and reviews, grouped by year. Most recent first.</p>

<div class="reviews-jumpnav">
Jump to:
{% for year_group in posts_by_year %}<a href="#{{ year_group.name }}">{{ year_group.name }}</a>{% unless forloop.last %} · {% endunless %}{% endfor %}
</div>

{% for year_group in posts_by_year %}<section class="reviews-year">
<h2 id="{{ year_group.name }}" class="reviews-year-heading">{{ year_group.name }}</h2>
<ul class="reviews-list">
{% for post in year_group.items %}<li><a href="{{ post.url | relative_url }}">{{ post.title }}</a></li>
{% endfor %}</ul>
</section>
{% endfor %}
