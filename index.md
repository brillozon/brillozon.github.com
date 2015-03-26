---
layout: page
title : Brillouin Zone
tagline: uniquely defining a primitive cell in reciprocal space
---
{% include JB/setup %}

## Things left to do

* Get search working for the site
* Create a sitemap for the site
* Get the styles in order for the pages and posts
* Make sure I get the attributions and licenses correct
* ... start posting... :D

## Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

