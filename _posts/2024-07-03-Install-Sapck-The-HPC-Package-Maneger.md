---
layout: post
title: "How To Install and Configure Spack -- The HPC Package Manager"
categories_short_name: shell
meta: "shell_and_OS"
type: "Draft"
---

[Spack](https://spack.io/), the supercomputer tool for HPC libraries and dependencies building, has been around quite a while in HPC community and almost appear on any big machine. It offers extremely comprehensive control of package building, such as the versions of target package, versions of dependencies of target package, compilers and their version. 

One of many significant features, which make Spack such handy tool for resolving HPC toolchains, is building configurations. [Spec variants](https://spack.readthedocs.io/en/latest/basic_usage.html#specs-dependencies) are the kernel concepts of this feature. `Spec variants` of spack package cover mostly the building configuration flags of [CMake](https://cmake.org/) or [Make](https://en.wikipedia.org/wiki/Make_(software)), the compiler flags and the linker flags. They are the ports of building configuration of softwares.

Beside appearing frequently on cluster of supercomputing nodes, Spack also works well on workstations and laptops. This makes deployment, managements and usages of HPC toolchains much more easier than it's used to be. For HPC developers, making this task easy is really life enjoyment. 

In this short article, I want to talk about how to fast install Spack from [Spack Github repo](https://github.com/spack/spack.git) on UNIX-like operating system such as FreeBSD and Debian/Ubuntu linux. I also will show a simple and necessary configuration to make Spack ready to go on daily usage. The resource I would like to quote here is [Spack documentation](https://spack.readthedocs.io/en/latest/getting_started.html#installation).

Installation and environment configurations are quite easy. First, `git clone` the repo in your preferred way. Either 
{% highlight console %}
$ git clone https://github.com/spack/spack.git ~/spack && cd ~/spack
$ git checkout releases/v0.20 # newest version is releases/v0.22 when this is written
{% endhighlight %}
or 
{% highlight console %}
$ git clone --depth=100 --branch=releases/v0.20 https://github.com/spack/spack.git ~/spack
$ cd ~/spack
{% endhighlight %}
would fetch [spack github repo](https://github.com/spack/spack) and checkout `releases/v0.20` branch in `/home/$USER/spack` folder.

Then we must let the operating system knows there is utilities of Spack by setting up environment variables. Spack package provides a series of shell scripts for different shells to automate this task. `ls` the `~/spack/share/spack/` path to see which one is your shell needs. Here, we use `bash` script `setup-env.sh` as example. To automate this regular task, we would put this line 
{% highlight console %}
$ . ~/share/spack/setup-env.sh
{% endhighlight %}
into `~/.bashrc` by running
{% highlight console %}
$ echo '. ~/share/spack/setup-env.sh' >> ~/.bashrc && source ~/.bashrc
{% endhighlight %}
After this, the installation is done. To test if spack works properly, we can try to run
{% highlight console %}
$ spack --version
{% endhighlight %}
to see the if version is right. Or do a more useful test by running 
{% highlight console %}
$ spack info hdf5 
{% endhighlight %}
to checkout which version of `hdf5` spack could offer, the building tools and dependencies, and what the `spec variants` are. If your installation and configuration were right, the output should very similar with this:
{% highlight console %}
xxxx@bsd:~/spack $ spack info hdf5
CMakePackage:   hdf5

Description:
    HDF5 is a data model, library, and file format for storing and managing
   ...

Homepage: https://portal.hdfgroup.org

Preferred version:  
    1.14.1-2         https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.14/hdf5-1.14.1-2/src/hdf5-1.14.1-2.tar.gz
    ...
Variants:
    Name [Default]          When                              Allowed values          Description
    ====================    ==============================    ====================    ============================================

    api [default]           --                                default, v116, v114,    Choose api compatibility for earlier version
                                                              v112, v110, v18, v16    
    build_system [cmake]    --                                cmake                   Build 
    ...
Build Dependencies:
    cmake  gmake  java  mpi  ninja  szip  zlib

Link Dependencies:
    mpi  szip  zlib

Run Dependencies:
    java  pkgconfig

{% endhighlight %}

- last time edited @18th. Aug. 2024