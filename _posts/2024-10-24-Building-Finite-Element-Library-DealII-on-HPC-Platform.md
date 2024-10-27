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
