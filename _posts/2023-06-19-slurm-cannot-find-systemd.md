---
layout:     post
title:      "Slurm on Gentoo: cannot find /sys/fs/cgroup/systemd"
subtitle:   "Slurm"
date:       2023-06-19 12:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Gentoo
    - Slurm
    - cgroup
    - network
---

## Slurm on Gentoo

Due to some reason, slurm doesn't work normally on Gentoo OS.

```bash
error: can not state /sys/fs/cgroup/systemd, no such file
```
This is because the cgroup initialization script did not take care of non-systemd(OpenRC) systems properly. 
One way to correct it is running the following command

```bash
mkdir -p /sys/fs/cgroup/systemd
mount -t cgroup -o none,name=systemd cgroup /sys/fs/cgroup/system
```

For slurm on Gentoo, it works well now. While, the following two command may be also needed for other applications
```bash
mkdir -p /sys/fs/cgroup/systemd/user
echo $$ > /sys/fs/cgroup/systemd/user/cgroup.procs 
```

Reference: [](https://github.com/moby/moby/issues/18922)

## Network on internal server

通过路由配置内网服务器通过公网服务器内网　ip 访问公网时，记得使　iptables 中的　masquarde（进行 snat 地址转换) 生效。
