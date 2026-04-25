---
title: Home
layout: home
nav_order: 1
---

# YoungJJu Blog

GitHub Pages and Just the Docs theme are ready.

## Latest Posts

{% assign posts = site.posts | sort: "date" | reverse %}
{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}

