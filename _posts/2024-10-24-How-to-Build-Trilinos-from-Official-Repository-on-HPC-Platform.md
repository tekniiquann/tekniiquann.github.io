---
layout: post
title: "How to Build Trilinos from Official Repository on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

[LLVM project](https://llvm.org), started by [Chris Lattner](https://en.wikipedia.org/wiki/Chris_Lattner) at 21 years ago, has been evolved to ubiquitous corner stone of almost any modern projects. Popular languages [Rust](https://www.rust-lang.org) and [Julia](https://julialang.org), which feature different capabilities, both heavily depend on LLVM for syntax analysis or binary generations (optimizations). However, getting our blue wyvern flying sometime needs a little bit more efforts, compiling and building LLVM from source is not same level task of a `apt install llvm`. 

<img src="/pictures/llvm-spack.png" alt="centered image" width="800" height="auto"> 

The problem gets seriously worse when it comes to supercomputer or cluster platforms, because these platforms usually have fixed number allocations of files in project folders. As a result, pulling source tree of LLVM into project folders is likely imposable thanks for its 180000+ files. Developers always found themselves have no way to conduct a source building of LLVM though they could get this job done on their workstation. It will be very helpful if an universal building system could provide a routine to properly fetch the source tree of LLVM, and could be even better if this building system offers detailed building configurations. Here, come to **Spack**. 

As been talked in my last article 
(<a href="{{ "/2024/07/03/Install-Spack-The-HPC-Package-Manager-and-Configure-its-Shell-Environment.html" | absolute_url }}">
Install Spack The HPC Package Manager and Configure its Shell Environment
</a>), 
Spack is designed to resolve this type of dependencies and toolchains building problem on multiple platforms. Though it totally doable to build up LLVM by following its [documentations](https://llvm.org/docs/GettingStarted.html#getting-the-source-code-and-building-llvm) and configuring the CMake, it's still **way more easier** for most users to just use the almost automated building tool. In this article, I will go through how to use Spack to easily and correctly build and install LLVM.

First, let's fetch installation and configuration information of LLVM. Different version of Spack releases provide different available version of LLVM, so do other packages. Generally speaking, the newer Spack is, the newer the packages it can offers. For example, Spack `release/v0.22` provides `llvm 18.0.x` installation, while Spack `release/v0.20` only up to `llvm 16.0.x`.

{% highlight console %}
~$ cmake  -DCMAKE_CXX_STANDARD=14 \
       -DCMAKE_C_COMPILER=cc \
       -DCMAKE_CXX_COMPILER=CC \
       -DCMAKE_Fortran_COMPILER=ftn \
       -DTrilinos_ENABLE_Xpetra=ON \
       -DTrilinos_ENABLE_Amesos=ON \
       -DTrilinos_ENABLE_Epetra=ON \
       -DTrilinos_ENABLE_EpetraExt=ON \
       -DTrilinos_ENABLE_Ifpack=ON \
       -DTrilinos_ENABLE_AztecOO=ON \
       -DTrilinos_ENABLE_Sacado=OFF \
       -DTrilinos_ENABLE_SEACAS=OFF \
       -DTrilinos_ENABLE_Teuchos=ON \
       -DTrilinos_ENABLE_Kokkos=ON \
       -DTrilinos_ENABLE_MueLu=OFF \
       -DTrilinos_ENABLE_ML=ON \
       -DTrilinos_ENABLE_ROL=OFF \
       -DTrilinos_ENABLE_Tpetra=ON \
       -DTrilinos_ENABLE_COMPLEX_DOUBLE=OFF \
       -DTrilinos_ENABLE_COMPLEX_FLOAT=OFF \
       -DTrilinos_ENABLE_Zoltan=ON \
       -DTrilinos_VERBOSE_CONFIGURE=OFF \
       -DTPL_ENABLE_MPI=ON \
       -DBUILD_SHARED_LIBS=ON \
       -DCMAKE_VERBOSE_MAKEFILE=ON \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DTPL_ENABLE_BLAS=ON  -DBLAS_LIBRARY_DIRS=/projappl/project_462000604/libs/TPL-BLAS \
       -DTPL_ENABLE_LAPACK=ON -DLAPACK_LIBRARY_DIRS=/projappl/project_462000604/libs/TPL-BLAS \
       -DTPL_ENABLE_Netcdf=OFF \
       -DCMAKE_INSTALL_PREFIX:PATH=/projappl/project_462000604/libs/Trilinos ../Trilinos-git
{% endhighlight %}

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
~$ spack spec -I llvm @17.0.6 %clang@16.0.6
{% endhighlight %}
`%` is the symbol for marking out compiler-to-use on operating system. While `@` symbol marks out the version of every library or tool involved. `@17.0.6` is version of LLVM I want to build in this example, and `@12.2.0` marks the version of `clang` I wish Spack to use. Compiler with given version must be executable on operating system. Try to run
{% highlight console %}
~$ spack compilers list 
{% endhighlight %}
to see what compilers have been detected by Spack on operating system. This command returns following contents
{% highlight console %}
==> Available compilers
-- clang debian12-x86_64 ----------------------------------------
clang@16.0.6  clang@15.0.6  clang@14.0.6  clang@13.0.1

-- gcc debian12-x86_64 ------------------------------------------
{% endhighlight %}
on `Debian 12`, in which `clang 13-16` and `gcc 12` are available and have successively detected by Spack.
If one wants to build `flang`, just assign `true` to `flang` after compiler specifier 
{% highlight console %} 
~$ spack spec -I llvm @17.0.6 %clang@16.0.6 flang=true
{% endhighlight %}

Now, let's take getting a little deeper into advance features of `spec`. We can write `spec` with very detailed information not only
about the package we want to build, but also very detailed `Variants` for the dependencies. Same with `Variants` of targeted package, one can
provide `Variants` of dependencies in the same command line of package `Spack` building. In this way, one can achieve very accurate control 
of the versions, the compiler-in-use and the dependencies of dependencies. For example, this `spec`
{% highlight console %}
~$ spack --debug spec -I llvm %clang@16.0.6 flang=true cuda=true cuda_arch=89 \ 
   ^cuda @11.0.2:12.4.0 %clang@16.0.6
{% endhighlight %}
returns 
{% highlight console %}
==> [2024-09-07-14:15:09.661897] '/usr/bin/clang-15' '-v' '/tmp/spack-implicit-link-info7jtqx34t/main.c' '-o' '/tmp/spack-implicit-link-info7jtqx34t/output'
...
Input spec
--------------------------------
 -   llvm%clang@16.0.6+cuda+flang cuda_arch=89
 -       ^cuda@11.0.2:12.4.0%clang@16.0.6

Concretized
--------------------------------
 -   llvm@14.0.6%clang@16.0.6+clang+cuda+flang+gold~ipo+libomptarget~libomptarget_debug~link_llvm_dylib+lld+lldb+llvm_dylib+lua~mlir+polly~python~split_dwarf~z3 build_system=cmake build_type=Release compiler-rt=runtime cuda_arch=89 generator=ninja libcxx=runtime libunwind=runtime openmp=runtime patches=1f42874,25bc503,6379168,8248141,b216cff,cb8e645 shlib_symbol_version=none targets=all version_suffix=none arch=linux-debian12-haswell
[+]      ^binutils@2.42%gcc@12.2.0~gas+gold~gprofng+headers~interwork+ld~libiberty~lto~nls~pgo+plugins build_system=autotools compress_debug_sections=zlib libs=shared,static arch=linux-debian12-haswell
[+]          ^diffutils@3.10%gcc@12.2.0 build_system=autotools arch=linux-debian12-haswell
...
[+]          ^curl@8.7.1%gcc@12.2.0~gssapi~ldap~libidn2~librtmp~libssh~libssh2+nghttp2 build_system=autotools libs=shared,static tls=openssl arch=linux-debian12-haswell
[+]              ^nghttp2@1.57.0%gcc@12.2.0 build_system=autotools arch=linux-debian12-haswell
 -       ^cuda@12.4.0%clang@16.0.6~allow-unsupported-compilers~dev build_system=generic arch=linux-debian12-haswell
 -       ^elfutils@0.190%clang@16.0.6~debuginfod+exeprefix+nls build_system=autotools arch=linux-debian12-haswell
[+]          ^bzip2@1.0.8%gcc@12.2.0~debug~pic+shared build_system=generic arch=linux-debian12-haswell
...
[e]      ^glibc@2.36%gcc@12.2.0 build_system=autotools arch=linux-debian12-haswell
[+]      ^hwloc@2.9.1%gcc@12.2.0~cairo~cuda~gl~libudev+libxml2~netloc~nvml~oneapi-level-zero~opencl+pci~rocm build_system=autotools libs=shared,static arch=linux-debian12-haswell
...
{% endhighlight %}
These information are verbose messages of sources and toolchains resolved from the `spec` we just provided, and information about dependencies which will be reused or installed freshly. Dependencies with `[+]` symbol in front are reused packages from previous Spack installations, while those with `[e]` are fetched from operating system. `cuda` with version `12.4` will be built if installation is launched with this `spec`. 

Option `--debug` is not necessary, but will be helpful when user has to deal with building errors. For developers, it's always good to see more details of processes than playing with black box. `^` in front of `cuda` marks the starting point of `spec`of CUDA building. Anything after this `^` symbol and before next `^` belongs to CUDA building configuration. One could specifies the version of CUDA, and also the compiler-in-use through same specifiers `%` and `@`. 
`:` symbol in `@12.0.0:17.0.6` of LLVM `spec` means any version between `12.0.0` and `17.0.6` will be fine. Similarly, in the `spec` of CUDA, `@11.0.2:12.4.0` means any version between `11.0.2` and `12.4.0` could be chosen.

This type of practice is absolutely necessary when user has to resolve the compatibility issue between different complex libraries, like here LLVM and CUDA.
The possible issues may rise from the un-compatible compilers, which different packages require for. Another common source of un-compatibility comes from different libraries, few of them may be just simply un-compatible with each others. The command line return from our command shows Spacks think LLVM version `14.0.6` has good compatibility with CUDA version `12.4` when they are both built with `Clang 16.0.6`.

At this stage, we have gathered all the knowledge for us to really build LLVM with CUDA support. If you are satisfied, then run
{% highlight console%}
~$ spack install -j4 --verbose llvm %clang@16.0.6 flang=true cuda=true cuda_arch=89 \ 
   ^cuda @11.0.2:12.4.0 %clang@16.0.6
{% endhighlight %}
to kick off the building with parallel compilations of threads and building `verbose`. Spack automatically supports parallel building for 
packages using `CMake` or `make`. The number of parallel CPU threads is `16` if the hardware has these resources. However, this number can always be overridden
by explicitly requiring `-j N`, which N is 4 in our example.

If you are not planing to get into rabbit hole of compatibility, or you just want a working LLVM for `AST` analysis or a newer version of `clang`, 
{% highlight console %}
~$ spack --debug spec -I llvm @17.0.6 %clang@16.0.6
...
~$ spack install -j4 --verbose llvm @17.0.6 %clang@16.0.6
{% endhighlight %}
will show what you need to know before building similar with we have seen before. And then launch the building. Variant `cuda` is default be `false` and variant `targets` is default be `x86` in this `spec`. For an old Intel 4th generation `i5-4300U` CPU, this building takes about two hours if any of the dependencies has not been installed ever. In contract,
This time is about 25 minutes on AMD `Zen3 EPYC 7003` CPU, thanks for the sixteen threads are automatically launched.

The built packages locate under `opt/spack/` folder of cloned Spack repo. Try to run 
{% highlight console %}
~$ spack find --path llvm
{% endhighlight %}
to check out the exact location. 
Under the install path, there are `bin` for LLVM excitable, such as `clang`, `llvm-*`, `include` for header files, and `lib` for library files. 
One may still gets errors and crashing in building process, debugging the `spec` you provided is necessary if this unfortunately happened. 

Thank you for reading and stay tuning for next article about **features** and **configurations** with `yaml` files on Spack if you find this article helpful and want find more advanced features of Spack.

- last time edited @7th. Sep. 2024