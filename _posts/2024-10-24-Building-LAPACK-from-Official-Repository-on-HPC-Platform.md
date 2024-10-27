---
layout: post
title: "Build LAPACK from Official Repository on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
~$ cmake -DCMAKE_C_COMPILER=cc \
         -DCMAKE_Fortran_COMPILER=ftn \
         -DCMAKE_BUILD_TYPE=Release \     
         -DCMAKE_C_FLAGS="-g" \  
         -DBUILD_SHARED_LIBS=ON \
         -DCMAKE_INSTALL_LIBDIR=/projappl/project_xxxxxx/libs/lapack ../lapack-git
{% endhighlight %}