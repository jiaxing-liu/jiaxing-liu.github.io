---
layout:     post
title:      "a solution to slow ssh connection"
subtitle:   "in Gentoo linux"
date:       2022-03-05 14:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
tags:
    - Linux
    - ssh
---

## the cause of a slow ssh connection
you can use ssh -v to chech why do you have a slow ssh connection
```bash
ssh -v username@server
```

If it is stucked on the step `pledge: exec`

```bash
debug1: channel 0: new [client-session]
debug1: Requesting no-more-sessions@openssh.com
debug1: Entering interactive session.
debug1: pledge: exec
```

then something the `elogind` or `systemd-logind` service maybe wrong. If so, restart the service can solve this problem.

## how to solve this problem

```bash
user@server ~ $ sudo rc-service elogind restart
 * Starting elogind ...
 * start-stop-daemon: /lib64/elogind/elogind is already running       [ !! ] * ERROR: elogind failed to start
```

  If you can not restart it, just kill and then start it.
