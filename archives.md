---
layout: page
title: 文章目录
permalink: /archives/
---
<ul>
  {% for post in site.posts %}
    <li>
	<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
    <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
