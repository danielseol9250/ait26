---
layout: page
title: "IT 블로그"
subtitle: "IT 기술과 개발에 관한 블로그"
---

<div class="posts-list">
  {% for post in site.posts %}
    <article class="post-preview">
      <a href="{{ post.url | relative_url }}">
        <h2 class="post-title">{{ post.title }}</h2>
        {% if post.subtitle %}
          <h3 class="post-subtitle">{{ post.subtitle }}</h3>
        {% endif %}
      </a>
      
      <p class="post-meta">
        Posted on {{ post.date | date: "%B %-d, %Y" }}
        {% if post.categories.size > 0 %}
          in {{ post.categories | join: ", " }}
        {% endif %}
      </p>
      
      {% if post.description %}
        <div class="post-entry-container">
          <p class="post-description">{{ post.description }}</p>
        </div>
      {% endif %}
      
      {% if post.tags.size > 0 %}
        <div class="post-tags">
          {% for tag in post.tags %}
            <a href="{{ '/tags/' | append: tag | relative_url }}">{{ tag }}</a>
          {% endfor %}
        </div>
      {% endif %}
    </article>
  {% endfor %}
</div>

