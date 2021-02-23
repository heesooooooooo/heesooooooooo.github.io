---
layout: page
cover: 'assets/images/cover5.jpg'
navigation: True
permalink: /tags/
title: Tags
---

{% if site.posts.size == 0 %}
  <h2>No post found</h2>
{% endif %}

<div class="tags">
  <ul class="label">
    {% assign sorted_tags = site.tags | sort %}
    {% for tag in sorted_tags %}
    <span>
      <a href="#{{ tag[0] }}">
        <span>{{ tag[0] }}</span>
        <span class="count">({{ tag[1] | size }})</span>
      </a>
    </span>
    {% endfor %}
  </ul>

  {% for tag in sorted_tags %}
    <h2 id="{{ tag[0] }}">
      🎃 {{ tag[0] }}
    </h2>
    <ul class="tag">
      {% for post in tag[1] %}
        {% if post.title != null %}
          <li>
            {% if post.link %}
              <a href="{{ post.link }}">
            {% else %}
              <a href="{{ site.baseurl }}{{ post.url | remove: '/'}}">
            {% endif %}
                {{ post.title }}
              </a>
              <time>{{ post.date | date: "%Y-%m-%d" }}</time>
          </li>
        {% endif %}
      {% endfor %}
    </ul>
  {% endfor %}
</div>
