---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

[![this site is viewable with any browser](/assets/badges/anybrowser3.gif)](https://anybrowser.org/campaign/)
[![i use arch btw](/assets/badges/archlinux.gif)](https://archlinux.org/)
[![brak now!](/assets/badges/braknow.gif)](https://www.cameo.com/andymerrill2)
[![tested on Firefox](/assets/badges/firefox4.gif)](https://www.firefox.com/en-US/)
[![fully automated luxury gay space piracy](/assets/badges/seedyourtorrents.gif)](#)
[![windows is dead and windows killed it](/assets/badges/stop.gif)](https://archlinux.org/)
[![i have literally never met anyone who uses emacs irl](/assets/badges/vim.vialle.love.anim.gif)](https://www.vim.org/)
[![remember who you are](/assets/badges/www.gif)](https://info.cern.ch/hypertext/WWW/TheProject.html)
![but i haven't survived 2038](/assets/badges/y2ks.gif)]

# Blog
{% capture _ %}{% increment blogcount %}{% endcapture %}
{% for post in site.posts %}
  {% if blogcount < 6 %}
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
  {% if newscount < 2 %}
    {% if post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.date | date_to_string }}</a></h2> 
{{ post.summary | default: post.excerpt }}
{% capture _ %}{% increment newscount %}{% endcapture %}
    {% endif %}
  {% endif %}
{% endfor %}


