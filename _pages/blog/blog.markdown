---
layout: blog
title: blog
permalink: /blog
---
# Blog
{% for post in site.posts %}
  {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> </h2> 
{{ post.date | date_to_string }}
<nav>[
    {% for tag in post.tags %}
	{% if tag != post.tags[0] %}, {% endif %}<a href="/blog/tags#{{ tag }}">{{ tag }}</a>
    {% endfor %}
]</nav>

{{ post.excerpt | default: post.summary }}
  {% endunless %}
{% endfor %}


<!--
{% for post in paginator.posts %}
  {% unless post.tags contains 'newsdump' %}
<h2><a href="{{ post.url }}" class="post-preview">{{ post.title }}</a> </h2> 
{{ post.date | date_to_string }}
<nav>[
    {% for tag in post.tags %}
	{% if tag != post.tags[0] %}, {% endif %}<a href="/blog/tags#{{ tag }}">{{ tag }}</a>
    {% endfor %}
]</nav>

{{ post.excerpt | default: post.summary }}
  {% endunless %}
{% endfor %}
-->
<!-- Pagination links -->
<!--
<div class="pagination">
  {% if paginator.previous_page %}
    <a href="{{ paginator.previous_page_path }}" class="previous">
      Previous
    </a>
  {% else %}
    <span class="previous">Previous</span>
  {% endif %}
  <span class="page_number ">
    Page: {{ paginator.page }} of {{ paginator.total_pages }}
  </span>
  {% if paginator.next_page %}
    <a href="{{ paginator.next_page_path }}" class="next">Next</a>
  {% else %}
    <span class="next ">Next</span>
  {% endif %}
</div>
-->
