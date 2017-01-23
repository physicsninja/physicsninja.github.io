---
layout: page
show_meta: false
title: "There is no title"
subheadline: "Buckle up"
#header:
#   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/wildcard/"
---
<ul>
    {% for post in site.categories.wildcard %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>