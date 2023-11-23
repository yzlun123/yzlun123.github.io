---
layout: page
title: Category 1
---

{% for post in site.categories.category1 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{% endfor %}
