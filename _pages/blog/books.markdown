---
layout: blog
title: "book reviews"
permalink: /blog/book-reviews
---
# Book Reviews

{% for post in site.posts %}
  {% if post.tags contains 'books' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> </h2> 
{{ post.excerpt | default: post.summary }}
  {% endif %}
{% endfor %}

