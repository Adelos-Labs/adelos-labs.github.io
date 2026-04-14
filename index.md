---
layout: default
title: Home
---

# Adelos Labs

Welcome. This is where I post notes, experiments, and writeups.

## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small>&mdash; {{ post.date | date: "%Y-%m-%d" }}</small>
    </li>
  {% endfor %}
</ul>
