---
layout:     post
title:      "Install gcc on CentOS without root priviligy"
subtitle:   "low version CentOS"
date:       2022-05-23 10:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - CentOS
    - gcc
---

# Install gcc-9.4.0 on CentOS without root priviligy

## prerequisites: GMP 4.2+, MPFR 2.4.0+ and MPC 0.8.0+
### GMP 4.2+

Download and extract the source file 
```bash
wget http://vault.centos.org/7.9.2009/os/Source/SPackages/gmp-6.0.0-15.el7.src.rpm
rpm2cpio gmp-6.0.0-15.el7.src.rpm |cpio -idv
tar xvf gmp-6.0.0a.tar.bz2 
```

Configure, build and install

```bash
mkdir build
.../src/configure/ --prefix=/home/jxliu/software/gmp/gmp-6.0.0
make -j20
make -j20 install
```

### MPFR 3.1.1

Download and extract the source file 

```bash
wget http://vault.centos.org/7.9.2009/os/Source/SPackages/mpfr-3.1.1-4.el7.src.rpm
rpm2cpio mpfr-3.1.1-4.el7.src.rpm|cpio -idv
tar xvf mpfr-3.1.1.tar.xz
```
Configure, build and install

```bash
mkdir build
.../src/configure/ --prefix=/home/jxliu/software/mpfr/mpfr-3.1.1
make -j20
make -j20 install
```

### MPC 3.1.1

Download and extract the source file 

```bash
  wget http://vault.centos.org/7.9.2009/os/Source/SPackages/libmpc-1.0.1-3.el7.src.rpm
  rpm2cpio libmpc-1.0.1-3.el7.src.rpm|cpio -idv
  tar xzvf mpc-1.0.1.tar.gz 
```
Configure, build and install

```bash
  mkdir build
  cd build/
  ../mpc-1.0.1/configure --prefix=/home/jxliu/software/mpc/mpc-1.0.1
  make -j20
  make -j20 install
```

## Configure, Build and install
Download and extract the source file

```bash
wget https://mirrors.ustc.edu.cn/gnu/gcc/gcc-9.4.0/gcc-9.4.0.tar.gz
tar xzvf gcc-9.4.0.tar.gz
```

Configuration, build and install

```bash
mkdir build
cd build
../gcc-9.4.0/configure --prefix=/home/jxliu/software/compilers/gcc-9.4.0 --with-gmp=/home/jxliu/software/gmp/gmp-6.0.0/ --with-mpfr=/home/jxliu/software/mpfr/mpfr-3.1.1/ --with-mpc=/home/jxliu/software/mpc/mpc-1.0.1/ --disable-multilib
make -j21
make -j21 install
```
## module file for gcc-9.4.0

contents in module file `gcc-9.4.0`
```bash
#%Module 1.0

set prefix /home/jxliu/software/compilers/gcc-9.4.0

prepend-path     CPLUS_INCLUDE_PATH      $prefix/include
prepend-path     C_INCLUDE_PATH          $prefix/include
prepend-path     LD_LIBRARY_PATH         $prefix/lib
prepend-path     LIBRARY_PATH            $prefix/lib
prepend-path     LD_LIBRARY_PATH         $prefix/lib64
prepend-path     LIBRARY_PATH            $prefix/lib64
prepend-path     PATH                    $prefix/bin
prepend-path     LD_RUN_PATH             $prefix/lib
prepend-path     LD_RUN_PATH             $prefix/lib64

## GMP
prepend-path     LD_LIBRARY_PATH         /home/jxliu/software/gmp/gmp-6.0.0/lib
prepend-path     LIBRARY_PATH            /home/jxliu/software/gmp/gmp-6.0.0/lib
prepend-path     LD_RUN_PATH             /home/jxliu/software/gmp/gmp-6.0.0/lib
prepend-path     CPLUS_INCLUDE_PATH      /home/jxliu/software/gmp/gmp-6.0.0/include
prepend-path     C_INCLUDE_PATH          /home/jxliu/software/gmp/gmp-6.0.0/include

## MPFR
prepend-path     LD_LIBRARY_PATH         /home/jxliu/software/mpfr/mpfr-3.1.1/lib
prepend-path     LIBRARY_PATH            /home/jxliu/software/mpfr/mpfr-3.1.1/lib
prepend-path     LD_RUN_PATH             /home/jxliu/software/mpfr/mpfr-3.1.1/lib
prepend-path     CPLUS_INCLUDE_PATH      /home/jxliu/software/mpfr/mpfr-3.1.1/include
prepend-path     C_INCLUDE_PATH          /home/jxliu/software/mpfr/mpfr-3.1.1/include

## MPC
prepend-path     LD_LIBRARY_PATH         /home/jxliu/software/mpc/mpc-1.0.1/lib
prepend-path     LIBRARY_PATH            /home/jxliu/software/mpc/mpc-1.0.1/lib
prepend-path     LD_RUN_PATH             /home/jxliu/software/mpc/mpc-1.0.1/lib
prepend-path     CPLUS_INCLUDE_PATH      /home/jxliu/software/mpc/mpc-1.0.1/include
prepend-path     C_INCLUDE_PATH          /home/jxliu/software/mpc/mpc-1.0.1/include

setenv           GCCHOME                 $prefix
setenv           CC                      "/home/jxliu/software/compilers/gcc-9.4.0/bin/gcc"
setenv           CXX                     "/home/jxliu/software/compilers/gcc-9.4.0/bin/g++"
setenv           FC                      "/home/jxliu/software/compilers/gcc-9.4.0/bin/gfortran"
setenv           F77                     "/home/jxliu/software/compilers/gcc-9.4.0/bin/gfortran"
```
