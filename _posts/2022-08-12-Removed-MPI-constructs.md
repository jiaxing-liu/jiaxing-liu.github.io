---
layout:     post
title:      "Removed MPI constructs"
subtitle:   "OpenMPI"
date:       2022-08-12 19:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - OpenMPI
    - Linux
---


# Removed MPI Constructs

Starting with v4.0.0, Open MPI — by default — removes the prototypes from mpi.h for MPI symbols that were deprecated in 1996 in the MPI-2.0 standard, and finally removed from the MPI-3.0 standard (2012).

Specifically, the following symbols (specified in the MPI language-neutral names) are no longer prototyped in mpi.h by default:

![Selection_003](https://user-images.githubusercontent.com/71710349/184339116-8a55998e-4805-44dd-b09c-6f08db10a065.png)


Although these symbols are no longer prototyped in mpi.h, _they are still present in the MPI library in Open MPI v4.0.x._ This enables legacy MPI applications to link and run successfully with Open MPI v4.0.x, even though they will fail to compile.

*The Open MPI team strongly encourages all MPI application developers to stop using these constructs that were first deprecated over 20 years ago, and finally removed from the MPI specification in MPI-3.0 (in 2012).* The FAQ items in this category show how to update your application to stop using these removed symbols.

All that being said, if you are unable to immediately update your application to stop using these removed MPI-1 symbols, you can re-enable them in mpi.h by configuring Open MPI with the --enable-mpi1-compatibility flag.


origin:[FAQ: Removed MPI constructs](https://www.open-mpi.org/faq/?category=mpi-removed)

