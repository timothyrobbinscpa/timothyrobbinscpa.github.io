---
layout: default
title: My Portfolio
---

<h1>{{ page.title }}</h1>
<ul>
  {% for project in site.portfolio %}
    <li>
      <h2><a href="{{ project.url }}">{{ project.title }}</a></h2>
      <p>{{ project.date | date_to_string }} - <img src="{{ project.image }}" alt="{{ project.title }}" style="width:100px;"></p>
      <p>{{ project.content | strip_html | truncatewords: 50 }}</p>
    </li>
  {% endfor %}
</ul>