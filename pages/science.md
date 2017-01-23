---
layout: page
show_meta: false
title: "Science and Math"
subheadline: "Semi-pedagogical writings of Neal Reynolds"
header:
   image_fullwidth: "header_unsplash_5.jpg"
permalink: "/science/"
---
<ul>
    {% for post in site.categories.science %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>