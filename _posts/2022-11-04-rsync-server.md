---
layout:     post
title:      "rsync server"
subtitle:   "mirrors"
date:       2022-11-04 22:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - rsync
    - mirror
---

## rsyncd configuration

`/etc/rsyncd.conf`

```bash
# /etc/rsyncd.conf

# Minimal configuration file for rsync daemon
# See rsync(1) and rsyncd.conf(5) man pages for help

# This line is required by the /etc/init.d/rsyncd script
pid file = /run/rsyncd.pid
log file = /var/log/rsyncd.log
use chroot = yes
read only = yes

# Simple example for enabling your own local rsync server
#[gentoo-portage]
#       path = /var/db/repos/gentoo
#       comment = Gentoo ebuild repository
#       exclude = /distfiles /packages
[sync]
        path            =/home/lug/mirrors/mirror_data/
        include from    =/home/lug/mirrors/mirror_data/rsync.list
[gentoo-portage]
        path            =/home/lug/mirrors/mirror_data/gentoo-portage
        include from    =/home/lug/mirrors/mirror_data/gentoo-portage.list
```

`rsync.list`

```bash
/home/lug/mirrors/mirror_data/***
```

## start server and test

```bash
rc-service rsyncd start
jxliu@mhd ~ $ rsync mhd.hust.edu.cn::
sync           
gentoo-portage
```
