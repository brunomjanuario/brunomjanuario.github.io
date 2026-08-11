---
layout: page
title: AI Fundamentals
permalink: /ai-fundamentals/
---

Daily study log on AI fundamentals.

<ul class="post-list">
  {% for post in site.categories.ai-fundamentals %}
  <li>
    <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
    <h3>
      <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
    </h3>
  </li>
  {% endfor %}
</ul>
