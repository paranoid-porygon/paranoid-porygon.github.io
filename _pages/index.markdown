---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

# Blog and News
{% for post in site.posts limit:5 %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> - {{ post.date | date_to_string }}</h2>
{{ post.excerpt | default: post.summary }}
{% endfor %}
