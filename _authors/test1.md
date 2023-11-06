---
layout: page
short_name: jill
name: Jill Smith
position: Chief Editor
---

Jill is an avid fruit grower based in the south of France.

<h2>Posts</h2>
<ul>
  {% assign filtered_posts = site.posts | where: 'author', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
