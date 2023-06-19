---
layout:     post
title:      "network problem due to mtu value"
subtitle:   "ssh problem"
date:       2022-08-28 20:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - SSH
    - mtu
    - Infiniband
---

In the following, I divide the problem finding and problem solving into several stages.

# 

## stage 1. hanging on when accessing nfs

nfs can be exported in the nfs server and can be mounted in the nfs client.[https://wiki.gentoo.org/wiki/Nfs-utils]

But it will hang on when connectting the client through `ssh` or accessing the nfs.

I still can excute command through the following method
```bash
ssh heq "ls /"
```

## stage 2. exporting and mounting nfs through normal network instead of ib network

If nfs is exported in the server and mounted in the client through normal network instead of infiniband network, ssh connection is successful and nfs is available.

So, there is something wrong about infiniband network. 

## stage 3. `ping` and `ssh` through ib network

next, it was found that `ping` works well through ib network. but ssh always fails through ib network.

Then the following command is used to find the root of the problem

```bash
ping -M do -s 100 ibio01
ping -M do -s 200 ibio01
ping -M do -s 300 ibio01
ping -M do -s 400 ibio01
```

it means the value of `mtu` of infiniband switch is too small.

## others
问题：`ssh connection stop at "debug1: SSH2_MSG_KEXINIT sent"`
解决方案： ib 网卡的 mtu 值需要一致(destination 的不能太大)
