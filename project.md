---
layout: page
title: Project
---

Here are my projects and related articles.

{% assign project_posts = site.categories.project %}

{% if project_posts.size > 0 %}
<ul class="post-list">
  {% for post in project_posts %}
  <li>
    <span class="post-meta">{{ post.date | date: "%b %-d, %Y" }}</span>
    <h3>
      <a class="post-link" href="{{ post.url | relative_url }}">
        {{ post.title | escape }}
      </a>
    </h3>
    {% if post.description %}
      <p>{{ post.description }}</p>
    {% else %}
      <p>{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
    {% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No project posts yet. Stay tuned!</p>
{% endif %}
