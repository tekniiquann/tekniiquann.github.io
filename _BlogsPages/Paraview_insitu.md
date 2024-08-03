---
layout: page
name: Paraview and Insitu visualization
short_name: paraview_Insitu
description: Real powerful and revelational HPC visualization tool kits. With the help of Paraview and insitu, messy csv text data sheets or slow python post-processing scripts will slowly out of your scope.
---

<img src="/pictures/paraview_insitu.png" alt="centered image" width="1500" height="auto"> 
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

