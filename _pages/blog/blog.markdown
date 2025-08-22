---
layout: blog
title: blog
permalink: /blog
---
# Blog
{% for post in site.posts %}
  {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> - {{ post.date | date_to_string }}</h2> 
{{ post.excerpt | default: post.summary }}
  {% endunless %}
{% endfor %}
