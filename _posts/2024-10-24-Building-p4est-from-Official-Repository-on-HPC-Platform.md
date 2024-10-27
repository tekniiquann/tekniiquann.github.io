---
layout: post
title: "Build p4est from Official Repository on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
~$ ../p4est-2.8.6/configure CC=cc \
                            CFLAGS="-Wall -Wuninitialized -O0 -g" \
                            --enable-mpi --prefix="/projappl/project_462000604/libs/p4est-2.8.6"

~$ make -j8 V=0
~$ make -j2 check V=0
~$ make -j10 install V=0                            
{% endhighlight %}

[p4est building tutorial](https://www.p4est.org/tutorial-build.html)