---
layout: default
title: Home
---

# Adelos Labs

Creating resources to support the use of [one-time pads](https://en.wikipedia.org/wiki/One-time_pad).

## Posts

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
      <small>&mdash; {{ post.date | date: "%Y-%m-%d" }}</small>
    </li>
  {% endfor %}
</ul>
