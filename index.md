---
layout: default
title: Home
---

# 환영합니다!

이곳은 OneStillForever의 기술 블로그입니다.

## 최근 글 목록

<ul>
{% for post in site.posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> - {{ post.date | date: "%Y-%m-%d" }}</li>
{% endfor %}
</ul>
