---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
title: Home
permalink: /
---

# Home

Welcome to the Dubious Disc Blog, my personal site. I maintain this site in the interest of POSSE (post own site; share everywhere) to try to dodge the algorithmic recommenders and censorship endemic to social media sites. You can find more info about this site and me at the [about page](/about).

Below you'll find the latest posts from my [personal weblog](/blog) as well as [links to news articles](/newsdump) that I think are worth your attention. I recommend [following](/feed.xml) [both](/newsfeed.xml) using your favorite RSS/atom feed reader.

## Latest from my blog
{% capture _ %}{% increment blogcount %}{% endcapture %}
{% for post in site.posts %}
  {% if blogcount < 6 %}
    {% unless post.tags contains 'newsdump' %}
<h3><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a></h3>
{{ post.summary | default: post.excerpt }}
{% capture _ %}{% increment blogcount %}{% endcapture %}
    {% endunless %}
  {% endif %}
{% endfor %}

<p>--</p>
[more bad posts >>](/blog)

## Latest reposts
{% capture _ %}{% increment postscount %}{% endcapture %}
{% assign posts = site.reposts | sort: 'date' | reverse %}
{% for post in posts %}
  {% if postscount < 6 %}
{% include repost.html post=post %}
{% capture _ %}{% increment postscount %}{% endcapture %}
  {% endif %}
{% endfor %}

<p>--</p>
[more poasts >>](/reposts)

## Latest news
{% capture _ %}{% increment newscount %}{% endcapture %}
{% assign news = site.newsdump | sort: 'date' | reverse %}
{% for post in news %}
  {% if newscount < 6 %}
{% include newspost.html post=post %}
{% capture _ %}{% increment newscount %}{% endcapture %}
  {% endif %}
{% endfor %}

<p>--</p>
[more noos >>](/newsdump)

## Blinkies

[![the www project](/assets/badges/www.gif)](https://info.cern.ch/hypertext/WWW/TheProject.html)
[![this site is viewable with any browser](/assets/badges/anybrowser3.gif)](https://anybrowser.org/campaign/)
[![i use arch btw](/assets/badges/archlinux.gif)](https://archlinux.org/)
[![i also use coreboot](/assets/badges/coreboot.gif)](https://www.coreboot.org/)
[![i have literally never met anyone who uses emacs irl](/assets/badges/vim.vialle.love.anim.gif)](https://www.vim.org/)
[![tested on Firefox](/assets/badges/firefox4.gif)](https://www.firefox.com/en-US/)
[![windows is dead and windows killed it](/assets/badges/stop.gif)](https://archlinux.org/)
[![remember to seed your torrents](/assets/badges/seedyourtorrents.gif)](#)
[![brak now!](/assets/badges/braknow.gif)](https://www.cameo.com/andymerrill2)
![i survived Y2K but i haven't survived 2038](/assets/badges/y2ks.gif)
[![yummy lil 88x31 badges](/assets/badges/88by31.gif)](https://cyber.dabamos.de/88x31/index.html)


