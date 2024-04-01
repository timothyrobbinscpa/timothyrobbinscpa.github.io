---
layout: archive
classes: wide
title: "Posts"
permalink: /posts/
author_profile: true
---
Dive deeper into my world of data science and finance through these detailed posts. Here, I provide comprehensive analyses of my projects, share insights from my experiences, and explore various concepts and techniques. Each post is a window into the methodology and thought process behind my work.

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>