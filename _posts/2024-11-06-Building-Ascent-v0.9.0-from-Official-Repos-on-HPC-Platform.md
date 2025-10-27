---
layout: post
title: "Build Ascent v0.9.x with Official Source on AMD Zen 3 CDNA 2 HPC System"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---
There are few well working parallel I/O frameworks for insitu and raw data streaming in heterogenous HPC ecosystem, 
such as [ADIOS 2](https://adios2.readthedocs.io/en/latest/index.html), [ParaView Catalyst](https://docs.paraview.org/en/latest/Catalyst/index.html) and [Ascent](https://ascent.readthedocs.io/en/latest/). Though ADIOS 2 has the widest adaption, 
Ascent is known for easy deploying. This short blog documents how to build and link Ascent on AMD system.  

### MPI Only
Ascent project provides a nice shell script to handle dependencies fetch and building under `scripts/build_ascent`, 
building MPI only library is as easy as passing parallel compiler warpers to `build_ascent.sh` script. 
{% highlight console %}
~$ module --force purge && module --force unload LUMI && module load LUMI/24.03 partition/C cpeGNU/24.03                         
{% endhighlight %}

There are lot of bugs on camp MEM for ascent dependencies, I have to manually add the missing headers.
 And few necessary modifications were done on build_ascent.sh to fetch v0.9.0 with all submodules, better document.
 just run this script in a empty folder, it will fetch all source codes:

{% highlight console %}
~$ env CC=cc CXX=CC FTN=ftn ./build_ascent-v0.9.0.sh
{% endhighlight %}

### ROCm-aware MPI

