---
layout: blog
title: blog
permalink: /blog
---
# Blog
{% for post in site.posts %}
  {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> </h2> 
{{ post.date | date_to_string }}
<nav>[
    {% for tag in post.tags %}
	{% if tag != post.tags[0] %}, {% endif %}<a href="/blog/tags#{{ tag }}">{{ tag }}</a>
    {% endfor %}
]</nav>

{{ post.excerpt | default: post.summary }}
  {% endunless %}
{% endfor %}
