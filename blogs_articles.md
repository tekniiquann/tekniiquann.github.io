---
layout: page
short_name: Blogs_Pages
title: Blogs & Articles
---


This page categorize tech blogs, articles and tutorials according to their topics. The common topics this website will involving are
#
 - `shell Script & OS` &mdash; "If there is shell, there is a way."
 - `C/C++, Compilers` &mdash; Languages for modern HPC; gcc/g++, LLVM/Clang, may be overlap with nvcc and hipcc.
 - `CUDA, ROCm/HIP, Kernel Language` &mdash; GPU accelerations and the kernel languages.
 - `Paraview and insitu Visualization` &mdash; insitu post-processing technologies.
 - `github.io and Jekyll` &mdash; Most popular static website service.
 - `Tech news which are generally interested`. &mdash; News from Phoronix, Hackers News.

<ul>
  {% for category in site.BlogsPages %}
    <li>
      <h2><a href="{{ category.url }}">{{ category.name }}</a></h2>
      <h3>{{ category.description }}</h3>
      <!---<p>{{ author.content | markdownify }}</p> --->
    </li>
  {% endfor %}
</ul>

