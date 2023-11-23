---
layout: page
title: Category 2
---

{% for post in site.categories.category2 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{% endfor %}