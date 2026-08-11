---
icon: fas fa-brain
order: 0
---

Daily study log on AI fundamentals.

<ul class="list-unstyled">
  {% assign posts = site.categories["AI Fundamentals"] %}
  {% for post in posts %}
  <li class="mb-2">
    <span class="text-muted small">{{ post.date | date: "%b %-d, %Y" }}</span>
    &mdash;
    <a href="{{ post.url | relative_url }}">{{ post.title | escape }}</a>
  </li>
  {% endfor %}
</ul>
