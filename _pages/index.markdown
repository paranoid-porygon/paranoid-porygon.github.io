---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

# Blog
{% for post in site.posts limit:5 %}
  {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> - {{ post.date | date_to_string }}</h2>
{{ post.summary | default: post.excerpt }}
  {% endunless %}
{% endfor %}

# Latest News
{% for post in site.posts limit:1 %}
  {% if post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.date | date_to_string }}</a></h2> 
{{ post.summary | default: post.excerpt }}
  {% endif %}
{% endfor %}
