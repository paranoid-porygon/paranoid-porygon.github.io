---
layout: blog
title: newsdump
permalink: /newsdump
---
# Newsdump
<!--{% for post in site.posts %}
  {% if post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.date | date_to_string }}</a></h2> 
{{ post.summary | default: post.excerpt }}
  {% endif %}
{% endfor %}

-->
{% for post in site.newsdump %}
  <h2><a href="{{ post.dest }}" class="post-preview">{{ post.title }}</a></h2>
{% endfor %}
