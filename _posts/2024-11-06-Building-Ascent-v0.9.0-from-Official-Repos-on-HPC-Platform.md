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
Ascent project provides a nice shell script to handle dependencies fetch and building automation. 
The script is under `scripts/build_ascent`, together with many other scripts working for specific HPC systems. 
building MPI only Ascent library is as easy as passing parallel compiler warpers to `build_ascent.sh` script. 
Before doing this, one should load necessary modules for `MPI`, `GCC/Clang` and `CMake`.
{% highlight console %}
~$ module --force purge && module --force unload PENV 
~$ module load PENV/xx.yy partition-info CompilerSuite/xx.yy BuildTools/xx.yy                         
{% endhighlight %}
Supposing `cc`, `C` and `ftn` are warpers of C, C++ and Fortran with MPI,
the following command will pass them to building script.
{% highlight console %}
~$ env CC=cc CXX=CC FTN=ftn ./build_ascent-v0.9.x.sh
{% endhighlight %} 

There are some bugs on `camp` and `FMEM` for Ascent dependencies in `v0.9.1-v0.9.2`, one has to manually add the missing headers when errors occur during compilation. And few necessary modifications were done on `build_ascent.sh` to fetch v0.9.x with all submodules. As one can find in building scripts of some well known Cray-AMD supercomputers, `enable_mpi` should be `OFF`, while `enable_find_mpi` should be `ON` after mpi module is loaded. 

### ROCm-aware MPI

### Link and Troubleshooting  

