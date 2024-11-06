---
layout: post
title: "Build Ascent v0.9.0 from Official Repository on Cray SUSELinux HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
~$ module --force purge && module --force unload LUMI && easybuildprefix 465 && module load LUMI/24.03 partition/C cpeGNU/24.03                         
{% endhighlight %}

There are lot of bugs on camp MEM for ascent depedencies, I have to manually add the missing headers.
 And few necessay modificatons were done on build_ascent.sh to fetch v0.9.0 with all submodules, better document.
 just run this script in a empty folder, it will fetch all source codes:

{% highlight console %}
~$ env CC=cc CXX=CC FTN=ftn ./build_ascent-v0.9.0.sh
{% endhighlight %}
