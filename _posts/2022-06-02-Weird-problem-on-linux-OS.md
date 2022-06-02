---
layout:     post
title:      "Weird problem on Gentoo linux"
subtitle:   "A local server - MHD"
date:       2022-06-02 24:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - Gentoo
    - spack
    - mhd(server)
---

# Weird

## spack

1. Error: Package 'gtensor' not found

```bash
Error: Package 'gtensor' not found
```
This error occures after I running `git pull` in the `spack_root` directory.

It is really weird. Finally, I remove the `~/.spack` directory, except the `~/.spack/linux/compilers.yaml` file, (Actually backup it) and regenerate it again through running `spack list`.

Then the error disappeared.
