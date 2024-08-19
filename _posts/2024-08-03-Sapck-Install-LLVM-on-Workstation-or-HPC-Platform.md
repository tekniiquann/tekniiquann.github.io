---
layout: post
title: "Use Spack Build and Install LLVM on Workstation or Supercomputer"
categories_short_name: shell
meta: "shell_and_OS"
type: "Draft"
---

[LLVM project](https://llvm.org), started by [Chris Lattner](https://en.wikipedia.org/wiki/Chris_Lattner) 21 years ago, has been evolved to ubiquitous corner stone of almost any modern projects. [Rust](https://www.rust-lang.org) and [Julia](https://julialang.org), the most popular languages which feature different capabilities, are both heavily depend on LLVM for syntax analysis or binary generations (optimizations). However, getting our blue wyvern flying needs a little bit efforts, compiling and building LLVM natively is not same level task of a `apt install llvm`. The problem gets seriously worse when it comes to supercomputer or cluster platforms because these platform usually has certainly mount of allocations of number of files, and developers always found themselves have no way to build LLVM though they could get job done on their workstation. Here, come to **Spack**. 

As been talked in my last article 
(<a href="{{ "/2024/07/03/Install-Spack-The-HPC-Package-Manager.html" | absolute_url }}">
How To Install and Configure Spack -- The HPC Package Manager
</a>), 
Spack is designed to resolve this type of dependencies and toolchains building problem on multiple platforms. Though it totally doable to build up LLVM by following its [documentations](https://llvm.org/docs/GettingStarted.html#getting-the-source-code-and-building-llvm) and configuring the CMake, it's still **way more easier** for most users to just use the almost automated building tool. In this article, I will go through how to use Spack to easily and correctly build and install LLVM.

First, fetch install and configuration information of LLVM. Different version of Spack releases provide different available version of LLVM, so do other packages. Generally speaking, the newer Spack is, the newer the packages it can offers. For example, Spack `release/v0.22` provides `llvm 18.0.x` installation, while Spack `release/v0.20` only up to `llvm 16.0.x`.
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
We can see the default building configuration system is [CMake](https://cmake.org) and building tool is [ninja](https://ninja-build.org) if CMake is used.