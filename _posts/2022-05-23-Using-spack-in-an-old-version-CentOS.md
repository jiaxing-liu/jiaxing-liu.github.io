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

## fetch error

Sometimes, there are error about fetch/SSL certificates.

```bash
==> Warning: Spack will not check SSL certificates. You need to update your Python to enable certificate verification.
==> Error: FetchError: All fetchers failed for spack-stage-gawk-5.1.1-gb4xundr3gdxyixn3olufyqrzlk3klvc

/home/jxliu/software/spack/lib/spack/spack/package.py:1537, in do_fetch:
       1534
       1535        self.stage.create()
       1536        err_msg = None if not self.manual_download else self.download_instr
  >>   1537        start_time = time.time()
       1538        self.stage.fetch(mirror_only, err_msg=err_msg)
       1539        self._fetch_time = time.time() - start_time
       1540
```

One simple method which may solve this problem is Using `--insecure`. This will not check SSL certificates and then fetch source code successfully.

As for how to remove the warning without using `--insecure`, there is a discussion on this webpage [](https://spack.readthedocs.io/en/latest/getting_started.html#compiler-configuration). But it did not work well for me.

## remove SSL check warnings

This warning may be caused by the use of lower version `git`, `curl`, `python`, `gcc`. So we first load high version `git`, `curl`, `python`, `gcc` and then load `spack`.

Here, I use spack install the newest curl, git, gcc@11.2.0.
```bash
module use /home/jxliu/software/spack/share/spack/modules/linux-centos7-skylake_avx512
module load  git-2.35.2-gcc-11.2.0-dgkovwu 
module load curl-7.83.0-gcc-11.2.0-rkxdfc4 
module load python-3.9.12-gcc-11.2.0-xcqi7hi
module load gcc-11.2.0-gcc-9.4.0-ypvyezp
. /home/jxliu/software/spack/share/spack/setup-env.sh
```

You can add the above shell scripts to the `~/.bashrc` file. 
