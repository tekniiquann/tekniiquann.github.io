---
layout: page
name: Interesting News
short_name: news
description: I'm a reader of Phoronix and Hackers News. These news let me know stages, news and issues about both software libraries and hardwares. For example, newly discovered vulnerabilities of Xorg rooted back to 1980s. Or benchmark of Raspberry Pi 5 against Pi 4.
---

<img src="/pictures/news.png" alt="centered image" width="1500" height="auto"> 
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
 