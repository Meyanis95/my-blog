---
layout: default
title: Blog
permalink: /blog/
---

<div style="max-width: 700px; margin: 0 auto;">
  <h1 style="font-size: 32px; font-weight: 700; margin-bottom: 48px; margin-top: 40px;">Writing</h1>

  <div class="post-list">
    {% for post in site.posts %}
      <div class="post-item">
        <h3><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h3>
        <div class="post-meta">{{ post.date | date: "%B %d, %Y" }}</div>
        <div class="post-excerpt">{{ post.excerpt | strip_html | truncatewords: 40 }}</div>
      </div>
    {% endfor %}
  </div>
</div>
