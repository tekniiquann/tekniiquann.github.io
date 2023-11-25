---
layout: page
name: github.io Liquid and Jekyll
categories: githubio_jekyll
description: A free website building platform provided by github, plenty of developers are using it include my self. Jekyll is backend framework of this service. It's worth that gitlab also provide very similar service called gitlab page.
---

<!---![githubio_pic](/pictures/github-pages.jpeg)--->
<img src="/pictures/github-pages.jpeg" alt="centered image" width="300" height="auto"> 
<img src="/pictures/jekyll-logo.png" alt="centered image" width="300" height="auto"> 
<img src="/pictures/html5.png" alt="centered image" width="300" height="auto">

---

<ul>
  {% assign filtered_posts = site.posts | where: 'categories', page.categories %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

<!---[Set up github.io website](/_posts/2023-10-16-set-up-github-page.md)---> 
