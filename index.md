---
title: Home
layout: home
nav_order: 1
---

# YoungJJu Blog

GitHub Pages, Jekyll, Just the Docs를 직접 연결하며 배운 과정을 기록합니다.

## Latest Posts

{% assign posts = site.posts | sort: "date" | reverse %}
{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}
