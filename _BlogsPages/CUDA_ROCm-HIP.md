---
layout: page
name: cuda rocm/hip and Kernel Language
short_name: cuda_hip_kernel_lang
description: Two mainstream libs of GPU accelerations provided by Nvidia and AMD. They are so-called kernel languages, which means acceleration is done by lunch kernel codes on GPU card's Stream Multiprocessors (SMs).
---

<img src="/pictures/kernel_lang.png" alt="centered image" width="1500" height="auto"> 
---
<ul>
  {% assign filtered_posts = site.posts | where: 'categories_short_name', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

