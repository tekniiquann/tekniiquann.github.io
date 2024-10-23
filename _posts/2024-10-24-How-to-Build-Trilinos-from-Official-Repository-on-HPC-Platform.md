---
layout: post
title: "How to Build Trilinos from Official Repository on HPC Platform"
categories_short_name: cc++_compilers
meta: "shell_and_OS"
type: "publication"
---

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
