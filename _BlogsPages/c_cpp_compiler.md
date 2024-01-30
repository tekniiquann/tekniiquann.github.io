---
layout: page
name: c/c++ & Compilers & dev-environment
short_name: cc++_compilers
description: C/C++ is The Language Family, which HPC require. Though memory security of C/C++ is always under debated, most of developers will keep using it because its efficiency, abundant frameworks and GPU acceleration supports. In most case, daily compilations are done by either gcc or LLVM based compilers. However, given the rapidly renewed standards of C++, even figuring out which version of g++ is getting complicated. In additional, hardware vendors also provide their compliers to user e.g., nvcc, hpicc, aocc etc. Code editor and IDE are also an interested and useful topic.
---

---
<ul>
  {% assign filtered_posts = site.posts | where: 'categories_short_name', page.short_name %}
  {% for post in filtered_posts %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
