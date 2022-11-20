---
layout:     post
title:      "docker large log file"
subtitle:   "docker"
date:       2022-11-04 22:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - docker
    - gitlab
---

## remove useless log file 

```bash
ls /var/lib/docker/containers/*/*-json.log -lh|awk '{print $9}'
echo  >  *.log
```
