---
layout: blog
title: blog
permalink: /blog
---

{% for post in site.posts %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> - {{ post.date | date_to_string }}</h2> 
{{ post.excerpt }}
{% endfor %}
