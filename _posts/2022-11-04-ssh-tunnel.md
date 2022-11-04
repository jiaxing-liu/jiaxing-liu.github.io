---
layout:     post
title:      "ssh tunnel"
subtitle:   "rsync"
date:       2022-11-04 21:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - ssh
    - tunnel
    - rsync
---

## transter data between two servers which are not directly connected.

```bash
ssh -f -L 1233:<target IP>:[ssh port] username@<transit server IP> sleep 10; rsync -auvzP -e 'ssh -p 1233' jxliu@127.0.0.1:/data/data12/ .
```
