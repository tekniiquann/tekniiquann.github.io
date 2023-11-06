---
layout: page
short_name: ted
name: Ted Doe
position: Writer
---

CTed has been eating fruit since he was baby.
c/c++

<h2>Posts</h2>
<ul>
  {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>