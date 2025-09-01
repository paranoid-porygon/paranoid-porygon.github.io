---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

![i use arch btw](/assets/badges/archlinux.gif)

# Blog
{% capture _ %}{% increment blogcount %}{% endcapture %}
{% for post in site.posts %}
  {% if blogcount < 5 %}
    {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a></h2>
{{ post.summary | default: post.excerpt }}
{% capture _ %}{% increment blogcount %}{% endcapture %}
    {% endunless %}
  {% endif %}
{% endfor %}

# Latest News
{% capture _ %}{% increment newscount %}{% endcapture %}
{% for post in site.posts %}
  {% if newscount < 1 %}
    {% if post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.date | date_to_string }}</a></h2> 
{{ post.summary | default: post.excerpt }}
{% capture _ %}{% increment newscount %}{% endcapture %}
    {% endif %}
  {% endif %}
{% endfor %}


