---
layout: post
title: "Building Finite Elements Library Deal.II from Repo on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "Draft"
---

{% highlight console %}
#!/bin/bash

cmake -DCMAKE_C_COMPILER=cc \
  -DCMAKE_CXX_COMPILER=CC \
  -DCMAKE_Fortran_COMPILER=ftn \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_FLAGS="-march=znver3 -g" \
  -DDEAL_II_CXX_FLAGS="-std=c++17" \
  -DDEAL_II_ALLOW_PLATFORM_INTROSPECTION=ON \
  -DDEAL_II_WITH_KOKKOS=OFF \
  -DDEAL_II_WITH_LAPACK=ON -DLAPACK_DIR="/projappl/project_462000604/libs/lapack" -DLAPACK_LIBRARIES="/projappl/project_462000604/libs/lapack/liblapack.so" \
  -DDEAL_II_WITH_MPI=ON \
  -DDEAL_II_WITH_P4EST=ON -DP4EST_DIR=/projappl/project_462000604/libs/p4est-2.8.6 \
  -DDEAL_II_WITH_SCALAPACK=ON -DSCALAPACK_DIR=/projappl/project_462000604/libs/ScaLAPACK \
  -DDEAL_II_WITH_TRILINOS=ON -DTRILINOS_DIR=/projappl/project_462000604/libs/Trilinos \
  -DCMAKE_INSTALL_PREFIX=/projappl/project_462000604/libs/dealii-9.4.0 ../dealii-git
{% endhighlight %}

I re-build `dealii` recently on `zen3` cluster with its updated and suggested toolchain environment. The default compilers are `gcc/13.2` and a customized  version of `clang 17.0`. So it's not good to use too old version of `dealii` like I've done on `zen2` cluster. I tried `dealii 9.4`, `dealii 9.5` and `dealii 9.6`, and ended on `9.5`. This doesn't mean `9.5` really is better than others, and in fact they are very similar.

I made a configuration shell script to make cmake configuration automatic, and few changes were made, here is the contents of script:
{% highlight console %}
#!/bin/bash

cmake -DCMAKE_C_COMPILER=cc \
  -DCMAKE_CXX_COMPILER=CC \
  -DCMAKE_Fortran_COMPILER=ftn \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_CXX_FLAGS="-march=znver3 -g" \
  -DDEAL_II_CXX_FLAGS="-std=c++17" \
  -DDEAL_II_ALLOW_PLATFORM_INTROSPECTION=ON \
  -DDEAL_II_WITH_KOKKOS=OFF \
  -DDEAL_II_WITH_COMPLEX_VALUES=OFF \
  -DDEAL_II_WITH_LAPACK=ON -DLAPACK_DIR="/projappl/project_462000604/libs/TPL-BLAS-LAPACK" -DLAPACK_LIBRARIES="/projappl/project_462000604/libs/TPL-BLAS-LAPACK/liblapack.so" \
  -DDEAL_II_WITH_MPI=ON \
  -DDEAL_II_WITH_P4EST=ON -DP4EST_DIR=/projappl/project_462000604/libs/p4est-2.8.6 \
  -DDEAL_II_WITH_SCALAPACK=ON -DSCALAPACK_DIR=/projappl/project_462000604/libs/ScaLAPACK \
  -DDEAL_II_WITH_TRILINOS=ON -DTRILINOS_DIR="/projappl/project_462000604/libs/Trilinos" \
  -DCMAKE_INSTALL_PREFIX=/projappl/project_462000604/libs/dealii-9.5 ../dealii-git
{% endhighlight %}
Similar to the case of Trilinos, I used same symbolic links of `cray_libsci` for `lapack` supporting without `mpi` or `òpenmp`. And `dealii-git` folder is clone with branch by
{% highlight console %}
$ git clone -b dealii-9.5 --single-branch https://github.com/dealii/dealii.git dealii-git
{% endhighlight %}

The toolchians of PE on `zen3` cluster has quite new `cmake/3.29.3` support, after successfully running confuguration script, one can run
{% highlight console %}
$ cmake --build . -j 16 --verbose
{% endhighlight %}
and 
{% highlight console %}
$ cmake --install .
{% endhighlight %}
to build and install to `ÌNATALL_PREFIX`.