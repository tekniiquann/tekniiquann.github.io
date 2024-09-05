---
layout: post
title: "Use Spack Build and Install LLVM on Workstation or Supercomputer"
categories_short_name: shell
meta: "shell_and_OS"
type: "Draft"
---

[LLVM project](https://llvm.org), started by [Chris Lattner](https://en.wikipedia.org/wiki/Chris_Lattner) at 21 years ago, has been evolved to ubiquitous corner stone of almost any modern projects. [Rust](https://www.rust-lang.org) and [Julia](https://julialang.org), the most popular languages which feature different capabilities, are both heavily depend on LLVM for syntax analysis or binary generations (optimizations). However, getting our blue wyvern flying sometime needs a little bit more efforts, compiling and building LLVM from source is not same level task of a `apt install llvm`. 

The problem gets seriously worse when it comes to supercomputer or cluster platforms, because these platforms usually have fixed number allocations of files in project folders. As a result, pulling source tree of LLVM into project folders is likely imposable thanks for its 180000+ files. Developers always found themselves have no way to conduct a source building of LLVM though they could get this job done on their workstation. It will be very helpful if an universal building system could provide a routine to properly fetch the source tree of LLVM, and could be even better if this building system offers detailed building configurations. Here, come to **Spack**. 

As been talked in my last article 
(<a href="{{ "/2024/07/03/Install-Spack-The-HPC-Package-Manager.html" | absolute_url }}">
How To Install and Configure Spack -- The HPC Package Manager
</a>), 
Spack is designed to resolve this type of dependencies and toolchains building problem on multiple platforms. Though it totally doable to build up LLVM by following its [documentations](https://llvm.org/docs/GettingStarted.html#getting-the-source-code-and-building-llvm) and configuring the CMake, it's still **way more easier** for most users to just use the almost automated building tool. In this article, I will go through how to use Spack to easily and correctly build and install LLVM.

First, let's fetch installation and configuration information of LLVM. Different version of Spack releases provide different available version of LLVM, so do other packages. Generally speaking, the newer Spack is, the newer the packages it can offers. For example, Spack `release/v0.22` provides `llvm 18.0.x` installation, while Spack `release/v0.20` only up to `llvm 16.0.x`.
{% highlight console %}
~$ spack info llvm
{% endhighlight %}
The output we want to put attentions on are `Variants` 
{% highlight console %}
...
Variants:
    Name [Default]                 When                              Allowed values          Description
    ===========================    ==============================    ====================    =================================================================================================

    build_system [cmake]           --                                cmake                   Build systems supported by the package
    build_type [Release]           [build_system=cmake]              Debug, Release,         CMake build type
                                                                     RelWithDebInfo,         
                                                                     MinSizeRel              
    clang [on]                     --                                on, off                 Build the LLVM C/C++/Objective-C compiler frontend
    code_signing [off]             [+lldb arch=darwin-None-None]     on, off                 Enable code-signing on macOS
    compiler-rt [on]               [+clang]                          on, off                 Build LLVM compiler runtime, including sanitizers
    cuda [off]                     --                                on, off                 Build with CUDA
    cuda_arch [none]               [+cuda]                           none, 11, 32, 70,       CUDA architecture
...
    flang [off]                    [@11:+clang]                      on, off                 Build the LLVM Fortran compiler frontend (experimental - parser only, needs GCC)
    generator [ninja]              [, build_system=cmake]            ninja                   the build system generator to use
...
{% endhighlight %}
We can see the default building configuration system is [CMake](https://cmake.org) and building tool is [ninja](https://ninja-build.org) if CMake is used. The default `build_type` is `Release`, which usually exactly is what we as an user want. We can also see `cuda` building is on building option list, while this building option surely will push the building time even longer if it is turned on.

Next, let's prepare the building by checking out what's Spack going to do, or what dependencies Spack plans to build and install. In Spack lingo, we should check out `spec` to get these information before kicking off the installations.
{% highlight console %} 
~$ spack spec -I llvm @17.0.6 %gcc@12.2.0
{% endhighlight %}
`%` is the symbol for making out compiler-to-use for building on operating system. While `@` symbol marks out the version of every library or tool involved. `@17.0.6` is version of LLVM I want to build in this example, and `@12.2.0` marks the version of `gcc` I wish Spack to use. Compiler with given version must be executable on living system. Try to run
{% highlight console %}
~$ spack compilers find 
{% endhighlight %}
to see what compilers have been detected by Spack on operating system.
If one wants to build `flang`, just assign `true` to `flang` after compiler specifier 
{% highlight console %} 
~$ spack spec -I llvm @17.0.6 %gcc@12.2.0 flang=ture
{% endhighlight %}