---
layout:     post
title:      "Install Parmetis using spack"
subtitle:   "spack"
date:       2022-07-11 20:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - CentOS
    - gcc
---

# Install `ParMERIS` using spack
```bash
  >> 17    CMake Error at /home/jxliu/software/spack/opt/spack/lin
           ux-centos7-skylake_avx512/gcc-11.2.0/cmake-3.23.1-bbc6p
           ttbzqq6ahmubfxj7spidrxhhha5/share/cmake-3.23/Modules/CM
           akeTestCCompiler.cmake:69 (message):
     18      The C compiler
     19
     20        "/home/jxliu/software/spack/opt/spack/linux-centos7
           -skylake_avx512/gcc-11.2.0/nvhpc-22.3-l3b5q6wj6icevhvid
           ihim46imenplnow/Linux_x86_64/22.3/comm_libs/mpi/bin/mpi
           cc"
     21
     22      is not able to compile a simple test program.
```

默认安装 `ParMETIS` 会有如上报错。

加上对 `openmpi` 的依赖可以编译成功，不再使用 `nvhpc` 编译器，而是使用 `openmpi`
```bash
spack install -j16  parmetis^openmpi/637tdhh
```
