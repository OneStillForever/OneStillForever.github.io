---
layout: page
title: Categories
permalink: /categories/
---

# 📂 카테고리별 글 모음

{% assign categories = site.categories | sort %}
{% for category in categories %}
  {% assign posts = category[1] %}
  {% assign category_name = category[0] %}
  
## 📁 {{ category_name | capitalize }} ({{ posts.size }})

<ul>
{% for post in posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <small>{{ post.date | date: "%Y-%m-%d" }}</small>
  </li>
{% endfor %}
</ul>

---
{% endfor %}

