---
layout: page
title: All posts
---

<div class="posts">
  {% for post in site.posts %}
    <h3 class="post-title">
      <a href="{{ site.baseurl }}{{ post.url }}">
        {{ post.title }}
      </a>
    </h3>
    <span class="post-date">{{ post.date | date_to_string }}</span>
    
  {% endfor %}
</div>
