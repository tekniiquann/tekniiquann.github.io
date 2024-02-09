---
layout: page
name: github.io Liquid and Jekyll
short_name: githubio_jekyll
description: A free website building platform provided by github, plenty of developers are using it include my self. Jekyll is backend framework of this service. It's worth that gitlab also provide very similar service called gitlab page.
---

<img src="/pictures/github-page.png" alt="centered image" width="1500" height="auto"> 
---

<ul>
  {% assign filtered_posts = site.posts | where: 'categories_short_name', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

