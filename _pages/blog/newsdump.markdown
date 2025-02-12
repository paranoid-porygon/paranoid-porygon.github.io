---
layout: blog
title: newsdump
permalink: /newsdump
---
# Newsdump
{% for post in site.posts %}
  {% if post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a></h2> 
{{ post.excerpt }}
  {% endif %}
{% endfor %}
