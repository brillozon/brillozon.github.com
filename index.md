---
layout: page
title : Brillozon
tagline: liberating blue smoke whenever possible
---
{% include JB/setup %}

## Upcoming Events

1.      StampedeCon 2015

        [Interactive Visualization in Human Time](http://stampedecon.com/sessions/interactive-visualization-in-human-time/)

## Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

