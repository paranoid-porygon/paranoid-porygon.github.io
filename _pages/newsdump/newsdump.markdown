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
{% assign news = site.newsdump | sort: 'date' | reverse %}
{% for post in news %}
  <p>{{ post.date | date_to_string }} - <a href="{{ post.dest }}" class="post-preview">{{ post.title }}</a></p>
{% endfor %}
