---
layout:     post
title:      "Install PSC code on the old CentOS"
subtitle:   "low version CentOS"
date:       2022-05-27 10:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - CentOS
    - PSC
    - spack
---

# Install PSC(Plasma Simulation Code) code on CentOS

## Using spack to manage almost all used packages

Go to these two records to see how using spack in an old Linux OS.
[Install gcc on CentOS without root privilegy/](https://jiaxing-liu.github.io/2022/05/23/Install-gcc-on-CentOS-without-root-privilegy/)
[Using spack in an old version CentOS](https://jiaxing-liu.github.io/2022/05/23/Using-spack-in-an-old-version-CentOS/)

## hdf5 problem

If there is an error like this. The HDF5 software with hl(high-level library) need to be installed.
```bash
CMake Error at /home/jxliu/software/spack/opt/spack/linux-centos7-skylake_avx512/gcc-11.2.0/cmake-3.23.1-bbc6pttbzqq6ahmubfxj7spidrxhhha5/share/cmake-3.23/Modules/FindHDF5.cmake:563 (get_target_property):
  get_target_property() called with non-existent target "hdf5_hl-shared".
Call Stack (most recent call first):
  src/libmrc/CMakeLists.txt:16 (find_package)
```

Using spack to install it.

```bash
spack install -j21 hdf5@1.12.1+hl
```

If one or more hdf5 have been installed, you can choose to install a different version hdf5.
