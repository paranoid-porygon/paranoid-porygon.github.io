---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

# Dubious Disk

  {% for post in site.posts %}
  <h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a></h2>
  {% endfor %}
