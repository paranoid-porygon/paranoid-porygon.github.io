---
layout: default
title: reposts
permalink: /reposts/
---

# Reposts

Here's stuff from other sites made by other people that I want to share with you.

{% assign news = site.reposts | sort: 'date' | reverse %}
{% for post in news %}
<!--
  {% assign currentyear = post.date | date: "%Y" %}
  {% assign currentmonth = post.date | date: "%B" %}
  {% if currentyear != year %}
<h2 id="{{ currentyear }}">{{ currentyear}}</h2>
    {% assign year = currentyear %}
  {% endif %}
  {% if currentmonth != month %}
<h3 id="{{ currentyear }}-{{ currentmonth }}">{{ currentmonth }}</h3>
    {% assign month = currentmonth %}
  {% endif %}
-->
{% include repost.html post=post %}
{% endfor %}
