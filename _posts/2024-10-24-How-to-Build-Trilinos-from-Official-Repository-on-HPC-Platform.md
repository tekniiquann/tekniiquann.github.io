---
layout: post
title: "How to Build Trilinos from Official Repository on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
~$ cmake  -DCMAKE_CXX_STANDARD=14    \
       -DCMAKE_C_COMPILER=cc      \
       -DCMAKE_CXX_COMPILER=CC    \
       -DCMAKE_Fortran_COMPILER=ftn \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DCMAKE_CXX_FLAGS="-g" \
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
       -DTrilinos_ENABLE_NOX=OFF \
       -DTrilinos_ENABLE_ROL=OFF\
       -DTrilinos_ENABLE_Tpetra=ON \
       -DTrilinos_ENABLE_COMPLEX_DOUBLE=OFF \
       -DTrilinos_ENABLE_COMPLEX_FLOAT=OFF \
       -DTrilinos_ENABLE_Zoltan=ON \
       -DTrilinos_ENABLE_Zoltan2=OFF \
       -DTrilinos_VERBOSE_CONFIGURE=OFF \
       -DTPL_ENABLE_Boost=OFF \
       -DTPL_ENABLE_MPI=ON \
       -DBUILD_SHARED_LIBS=ON \
       -DCMAKE_VERBOSE_MAKEFILE=ON \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DTPL_ENABLE_BLAS=ON  -DBLAS_LIBRARY_DIRS=/projappl/project_xxxxxxxx/libs/lapack \
       -DTPL_ENABLE_LAPACK=ON -DLAPACK_LIBRARY_DIRS=/projappl/project_xxxxxxxx/libs/lapack \
       -DTPL_ENABLE_Netcdf=OFF \
       -DCMAKE_INSTALL_PREFIX:PATH=/projappl/project_xxxxxxxxx/libs/Trilinos ../Trilinos-git
{% endhighlight %}

The above is a shell command with self-built LAPACK support. To make life easier, one would put this long command into a shell script.
In new building on a `zen3` cluster, I used `CXX_STANDARD=17` other than `14`. Moreover, because a suspicious warning/error message popes up when I was building `LAPACK`, I used `Cray_libsci` on Cray cluster to get `lapack` support. `Cray_libsci` in SUSE Linux of `zen3` cluster has file name like `libsci_comp_mpi.a(.so)`dependent on if it's `mpi`, `openmp` or normal version. I made symbolic link pointing to these files with name like `liblapack.so` and `libblas.so` in a folder named `TPL-BLAS-LAPACK`.

So here is the contents in script `Trilinos-conf.sh`:
{% highlight console %}
#!/bin/bash

cmake  -DCMAKE_CXX_STANDARD=17    \
       -DCMAKE_C_COMPILER=cc      \
       -DCMAKE_CXX_COMPILER=CC    \
       -DCMAKE_Fortran_COMPILER=ftn \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DCMAKE_CXX_FLAGS="-g" \
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
       -DTrilinos_ENABLE_NOX=OFF \
       -DTrilinos_ENABLE_ROL=OFF\
       -DTrilinos_ENABLE_Tpetra=ON \
       -DTrilinos_ENABLE_COMPLEX_DOUBLE=OFF \
       -DTrilinos_ENABLE_COMPLEX_FLOAT=OFF \
       -DTrilinos_ENABLE_Zoltan=ON \
       -DTrilinos_ENABLE_Zoltan2=OFF \
       -DTrilinos_VERBOSE_CONFIGURE=OFF \
       -DTPL_ENABLE_Boost=OFF \
       -DTPL_ENABLE_MPI=ON \
       -DBUILD_SHARED_LIBS=ON \
       -DCMAKE_VERBOSE_MAKEFILE=ON \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DTPL_ENABLE_BLAS=ON  -DBLAS_LIBRARY_DIRS=/projappl/project_462000604/libs/TPL-BLAS-LAPACK \
       -DTPL_ENABLE_LAPACK=ON -DLAPACK_LIBRARY_DIRS=/projappl/project_462000604/libs/TPL-BLAS-LAPACK \
       -DTPL_ENABLE_Netcdf=OFF \
       -DCMAKE_INSTALL_PREFIX:PATH=/projappl/project_462000604/libs/Trilinos ../Trilinos-git
{% endhighlight %}
`Trilinos-git` is `github` clone of Trilinos, I use 
{% highlight console %}
$ git clone -b trilinos-release-14-4-branch --single-branch https://github.com/trilinos/Trilinos.git Trilinos-git
{% endhighlight %}
to directly clone-checkout certain branch. 

I actually tried different versions such as `13.4`,`14.0`, `14.2`,`14.4`,`15.0` and `16.0` with `gcc/13.2`.
It must be pointed out that `13.4` and `14.0` are really out of data against `gcc/13.2`, they generate a lot of errors during compilations.
In comparing, I have perfectly working `12.18` build with `gcc/11.2.0` on a `zen2` cluster.
Here is a similar issue with 14.0 on `gcc/13.1` build reported at Trilinos's gihub repo: [https://github.com/trilinos/Trilinos/issues/11923](https://github.com/trilinos/Trilinos/issues/11923). Finally `ǜ14.4` is under using.

`Trilinos 16` or later version is also seemly unusable because `ML` supporting will be dropped, see release note: [https://github.com/trilinos/Trilinos/releases/tag/trilinos-release-16-0-0](https://github.com/trilinos/Trilinos/releases/tag/trilinos-release-16-0-0).