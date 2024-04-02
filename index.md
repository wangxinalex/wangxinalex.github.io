---
layout: default
title: A Myriad of Stars
---

# Welcome to my blog

<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
  </li>
{% endfor %}
</ul>