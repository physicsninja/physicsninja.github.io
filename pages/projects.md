---
layout: page
show_meta: false
title: "I made stuff"
subheadline: "Hopefully it's useful to you"
#header:
#   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/projects/"
---
<ul>
    {% for post in site.categories.projects %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>