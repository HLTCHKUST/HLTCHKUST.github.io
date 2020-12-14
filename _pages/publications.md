---
layout: page
permalink: /publications/
title: Publications
description: Lists of publications from our lab, ordered by date.
years: [2020, 2019, 2018, 2017, 2016]
nav: true
---

<div class="publications">

{% for y in page.years %}
  <h2 class="year" style="color: #757575">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
