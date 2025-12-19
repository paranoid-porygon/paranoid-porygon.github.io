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
  {% assign currentyear = post.date | date: "%Y" %}
  {% assign currentmonth = post.date | date: "%B" %}
  {% if currentyear != year %}
<h2>{{ currentyear}}</h2>
    {% assign year = currentyear %}
  {% endif %}
  {% if currentmonth != month %}
<h3>{{ currentmonth }}</h3>
    {% assign month = currentmonth %}
  {% endif %}
{% include newspost.html post=post %}
{% endfor %}
