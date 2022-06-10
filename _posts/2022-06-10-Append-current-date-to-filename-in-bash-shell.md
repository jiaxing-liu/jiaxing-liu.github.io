---
layout:     post
title:      "Append current date to filename in bash shell"
subtitle:   "skill"
date:       2022-06-10 10:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - Gentoo
    - shell
---


# Append current date to filename in bash shell

## A case

```bash
mpirun -n 4 psc_harris_xz >> "psc_harris_xz-`date +"%m-%d-%y"`.log"
```
