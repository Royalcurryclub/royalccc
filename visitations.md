---
title: Visitations
layout: table-page
permalink: /visitations/
---

<table>
  <thead>
    <tr>
      <th>#</th>
      <th>When</th>
      <th>Curry Establishment</th>
      <th>Where</th>
      <th>Before/After Drinks</th>
    </tr>
  </thead>
  <tbody>
    {%- for v in site.data.visitations.visitations -%}
    <tr>
      <td>{{ v.sitting }}</td>
      <td>{{ v.when }}</td>
      <td>{{ v.curry }}</td>
      <td>{{ v.where }}</td>
      <td>{{ v.drinks }}</td>
    </tr>
    {%- endfor -%}
  </tbody>
</table>
