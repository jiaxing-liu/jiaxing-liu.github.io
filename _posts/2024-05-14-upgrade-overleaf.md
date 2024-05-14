---
layout:     post
title:      "Upgrade the sharelatex container"
subtitle:   "sharelatex in docker"
date:       2024-05-14 20:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
tags:
    - Linux
---

##升级 sharelatex

升级时遇到以下错误 
```bash
Mongo instance doesn't support transactions
```
通过在 `docker-compose.yml` 中加入 `command: --replSet rs0` 解决。
```bash
# docker-compose.yml
mongo:
        restart: always
        image: mongo:4.4
        command: --replSet rs0
```
