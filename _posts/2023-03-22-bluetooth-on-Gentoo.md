---
layout:     post
title:      "Connecting bluetooth device on Gentoo OS"
subtitle:   "Word"
date:       2020-02-04 16:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Bluetooth
    - Gentoo
    - Linux
    - OpenRC
---

## Enable bluetooth on Gentoo OS

```bash
bluetoothctl
[bluetooth]# list
Controller 8C:88:2B:44:3C:18 BlueZ 5.66 [default]
[bluetooth]# show 8C:88:2B:44:3C:18
[bluetooth]# power on
[bluetooth]# agent on
[bluetooth]# discoverable on
```

如果遇到 

```bash
Failed to set discoverable on: org.bluez.Error.Busy
```

或者类似的错误，可以尝试先停掉蓝牙服务，再打开

```bash
rc-service bluetooth stop
rc-service bluetooth start
```
