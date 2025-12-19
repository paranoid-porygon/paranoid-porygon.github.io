---
layout: blog
title: tags
permalink: /newsdump/tags
---
{% assign sortedTags = (site.tags | sort:0) %}
{% for tag in sortedTags %}
{% unless tag[0] == 'newsdump' %}
<h2 id="{{ tag[0] }}">{{ tag[0] }}</h2>
<ul>
  {% for post in tag[1] %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> — {{ post.date | date_to_string }}</li>
  {% endfor %}
</ul> 
{% endunless %}
{% endfor %}

