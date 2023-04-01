---
layout:     post
title:      "Picgo+gitlab+typora"
subtitle:   "typora"
date:       2023-04-01 12:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Picgo
    - gitlab
    - typora
    - markdown
---

# Picgo+gitlab+typora

## On gentoo OS

Install necessary software

```bash
sudo emerge --ask net-libs/nodejs
sudo npm install -g picgo
picgo -v
```



```bash
picgo install gitlab-files
picgo use
picgo config uploader
```

test

```bash
picgo u test.png
```

## On arch/manjaro OS

Install necessary software

```bash
sudo pacman -S nodejs npm
sudo npm install -g picgo
picgo -V
```

