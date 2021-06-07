---
layout: archive
title: "Portfolio"
date: 2021-06-07T11:39:03-04:00
modified:
excerpt: "Proyectos personales centrados en Data Science, pero abiertos a nuevos campos"
tags: []
image: DataScience.jpg
  feature:
  teaser:
---

<div class="tiles">
{% for post in site.categories.articles %}
  {% include post-grid.html %}
{% endfor %}
</div><!-- /.tiles -->
