---
layout:     post
title:      "Shared folder on Linux"
subtitle:   "Word"
date:       2020-04-11 12:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - share
    - Gentoo
    - Linux
    - File transfer
---

## 创建文件夹，用作共享
```bash
sudo mkdir /share
```
方便管理文件/文件夹所有权，创建名为 `shared` 的新用户组，将需要访问共享文件夹的用户添加到该组中

```bash
sudo groupadd shared
sudo usermod -aG shared username
```

更改共享文件夹所属组为 `shared`,并设置 `2770` 权限,该权限保证了新创建的文件和目录也属于 `shared` 组
```bash
sudo chown :shared /share
sudo chmod 2770 /share
```
