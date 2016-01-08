---
layout: page
title : Brillozon
tagline: liberating blue smoke whenever possible
---
{% include JB/setup %}

## Upcoming Events

1.      1/13/16  [OCI and ITEN hosting Open Source event](http://interact.stltoday.com/pr/business/PR010416120224364)

## Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

