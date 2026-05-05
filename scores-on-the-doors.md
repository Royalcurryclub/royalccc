---
title: Scores on the Doors
layout: table-page
show_average_score: true
permalink: /scores-on-the-doors/
---

<table>
  <thead>
    <tr>
      <th>When</th>
      <th>Visitation</th>
      <th>Service /5</th>
      <th>Food /5</th>
      <th>Total /10</th>
    </tr>
  </thead>
  <tbody>
    {%- for s in site.data.scores -%}
    <tr>
      <td>{{ s.when }}</td>
      <td>
        {%- if s.review_url and s.review_url != "" -%}
          <a href="{{ s.review_url }}">{{ s.visitation }}</a>
        {%- else -%}
          {{ s.visitation }}
        {%- endif -%}
      </td>
      <td>{{ s.service }}</td>
      <td>{{ s.food }}</td>
      <td>{{ s.total }}</td>
    </tr>
    {%- endfor -%}
  </tbody>
</table>
