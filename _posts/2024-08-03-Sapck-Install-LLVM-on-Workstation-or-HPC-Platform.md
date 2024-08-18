---
layout: post
title: "Use Spack Build and Install LLVM on Workstation or Supercomputer"
categories_short_name: shell
meta: "shell_and_OS"
type: "Draft"
---

[LLVM project](https://llvm.org), started by [Chris Lattner](https://en.wikipedia.org/wiki/Chris_Lattner) 21 years ago, has been evolved to ubiquitous corner stone of almost any modern projects. [Rust](https://www.rust-lang.org) and [Julia](https://julialang.org), the most popular languages which feature different capabilities, are both heavily depend on LLVM for syntax analysis or binary generations (optimizations). However, getting our blue wyvern flying needs a little bit efforts, compiling and building LLVM natively is not same level task of a `apt install llvm`. The problem gets seriously worse when it comes to supercomputer or cluster platforms because these platform usually has certainly mount of allocations of number of files, and developers always found themselves have no way to build LLVM though they could get job done on their workstation. Here, come to **Spack**. 

As been talked in my last article, [How To Install and Configure Spack -- The HPC Package Manager]({{/_posts/2024-07-03-Install-Sapck-The-HPC-Package-Maneger.md | absolute_url}}), Spack is designed to resolve this type of dependencies and toolchains building problem on multiple platforms. Though it totally doable to build up LLVM by following its [documentations](https://llvm.org/docs/GettingStarted.html#getting-the-source-code-and-building-llvm) and configuring the CMake, it's still **way more easier** for most users to just use the almost automated building tool. In this article, I will go how to use Spack to easily and correctly build and install LLVM.