---
layout: page
title : Brillozon
tagline: liberating blue smoke whenever possible
---
{% include JB/setup %}

## Upcoming Events

1.      WUHACK (Washington University Hack-a-thon)

        I will be working for a few hours on Saturday.

        [http://wuhack.com/](http://wuhack.com/)

2.      Strangeloop 2015

        I will be attending.  :)

        [http://www.thestrangeloop.com/](http://www.thestrangeloop.com/)

## Posts
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

