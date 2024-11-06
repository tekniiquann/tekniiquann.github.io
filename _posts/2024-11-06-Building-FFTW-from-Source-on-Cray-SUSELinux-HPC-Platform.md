---
layout: post
title: "Build FFTW from Source on Cray SUSELinux HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
~$ module --force purge && module --force unload LUMI && easybuildprefix 465 && module load LUMI/24.03 partition/C cpeGNU/24.03
{% endhighlight %}

{% highlight console %}
~$ ./configure CC=cc CFLAGS="-O2 -funroll-loops -march=znver3" --prefix=/projappl/project_462000465/libs/fftw-3.3.10 --enable-float --enable-sse --enable-sse2 --enable-avx --enable-avx2 --enable-shared     
~$ make
~$ make install                
{% endhighlight %}

Source code download: https://fftw.org/download.html

build and test instruction: https://fftw.org/fftw3_doc/Installation-on-Unix.html

precision options https://fftw.org/fftw3_doc/Precision.html. notice if you build --enable-float, then avx should be disable.

{% highlight console %}
~$ make check
{% endhighlight %}


[p4est building tutorial](https://www.p4est.org/tutorial-build.html)