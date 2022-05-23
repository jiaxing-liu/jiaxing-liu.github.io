---
layout:     post
title:      "Using spack in an old version CentOS"
subtitle:   "low version CentOS"
date:       2022-05-23 11:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - CentOS
    - spack
---

# Using spack in a old version CentOS

## Load newer version gcc, cmake, python(anaconda), etc...

```bash
module load cmake-3.21.1
module load anaconda3
module load gcc-9.4.0
```
## clear the spack cache and start over

```bash
rm ~/.spack -rf
spack compiler find
spack spec gcc@11.2.0
spack install -j21 gcc@11.2.0
```
