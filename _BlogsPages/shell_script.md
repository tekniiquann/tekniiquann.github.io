---
layout: page
name: Shell Scripts & UNIX-like OS
short_name: shell
description: One of greatest innovations among UNIX/Linux is programable shell.A functional shell script always makes simple tasks simple and hard tasks simpler. Meanwhile, Plenty of UNIX-like OS distros including BSD and Linux have been released and iterated many generations. It's helpful to discuss them and document troubleshooting.
---

<img src="/pictures/shell.png" alt="centered image" width="1500" height="auto"> 
---
<ul>
  {% assign filtered_posts = site.posts | where: 'categories_short_name', page.short_name %}
  {% for post in filtered_posts %}
    {%- if post.type != "Draft" -%}
          <li>
            <a href="{{ post.url }}">{{ post.title }}</a>
          </li>
    {%- endif -%}
  {% endfor %}
</ul>
