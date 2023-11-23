---
layout: page
title: Category 3
---

{% for post in site.categories.category3 %}
  <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
{% endfor %}