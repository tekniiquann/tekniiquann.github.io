---
layout: page
short_name: ted
name: Ted Doe
description: One of greatest innovations among UNIX/Linux is programable shell.A functional shell script always makes simple tasks simple and hard tasks simpler. Meanwhile, Plenty of UNIX-like OS distros including BSD and Linux have been released and iterated many generations. It's helpful to discuss them and document troubleshooting.
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