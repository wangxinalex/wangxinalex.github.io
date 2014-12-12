---
layout: page
title: Alex Lightblog
tagline: Supporting tagline
---
{% include JB/setup %}


<ul class="posts">
  {% for post in site.posts %}
    <li class="index"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
	<div class="excerpt">{{post.excerpt | strip_html | truncatewords: 100}}</div></li>
  {% endfor %}
</ul>
