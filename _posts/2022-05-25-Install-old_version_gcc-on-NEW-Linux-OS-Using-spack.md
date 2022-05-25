---
layout:     post
title:      "Install old version gcc Using spack"
subtitle:   "low version CentOS"
date:       2022-05-25 10:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - CentOS
    - gcc
    - Gentoo
    - spack
---


# Install old version gcc Using spack

## System info

output of `uname -a`

```bash
Linux heq 5.15.32-gentoo-r1-x86_64 #1 SMP Tue May 10 17:04:20 CST 2022 x86_64 Intel(R) Xeon(R) Gold 5215 CPU @ 2.50GHz GenuineIntel GNU/Linux
```

`gcc@11.2.0` and `gcc@7.5.0` can be installed successfully in a easy way.

```bash
spack install gcc@11.2.0
spack install gcc@7.5.0
```
## Problem and solution

However, there will be some errors when installing `gcc@4.8.5`, like

```bash
 >> 16572    /tmp/root/spack-stage/spack-stage-gcc-4.8.5-cc7pwaiv5zxfdqo4gwg3fyho2kxkjicd/spack-src/gcc/reload1.c:
              89:24: error: use of an operand of type 'bool' in 'operator++' is forbidden in C++17
```

```bash
==> Error: ProcessError: Command exited with status 2:
>> 20919    xgcc: error trying to exec 'cc1': execvp: No such file or directory
```

```bash
 >> 23488    /tmp/root/spack-stage/spack-stage-gcc-4.8.5-5d2d7ynqm72ugjv3eyfh6jzwsw6eh2pg/spack-src/libsanitizer/a
              san/asan_linux.cc:95:20: error: 'SIGSEGV' was not declared in this scope
```

Then, I google it and find a solution here [issue on spack-github](https://github.com/spack/spack/issues/15229)

First, there need several patchs: gcc-backport.patch, ucontext\_t-4.8.patch, signal.patch, stack_t-4.8.patch. Some (gcc-backport.patch and signal.patch) are already exist in spack (`.../var/spack/repos/builtin/packages/gcc`), and the other need to be download from other place.

Then, these three lines need to be added the `package.py` file, located in `.../var/spack/repos/builtin/packages/gcc`
```bash
    patch('ucontext_t-4.8.patch', when='@4.8')
    patch('signal.patch', when='@4.8')
    patch('stack_t-4.8.patch', when='@4.8')
```

Finally, I install `gcc@4.8.5` successfully using `gcc@7.5.0` with low version `texinfo` and `gmp`, also with `LD_PRELOAD=/usr/lib/gcc/x86_64-pc-linux-gnu/11.2.1/libstdc++.so.6`.

```bash
LD_PRELOAD=/usr/lib/gcc/x86_64-pc-linux-gnu/11.2.1/libstdc++.so.6 spack install -j21 gcc@4.8.5  %gcc@11.2.0 ^texinfo@6.0 ^gmp@5.1.3
```

