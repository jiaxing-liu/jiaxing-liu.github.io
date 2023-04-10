---
layout:     post
title:      "FTP server on Gentoo OS"
subtitle:   "Word"
date:       2020-04-10 16:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - ftp
    - Gentoo
    - Linux
    - File transfer
---

## Install `vsftpd` on Gentoo

```bash
emerge -av vsftpd
```

## configurations
```bash
# file /etc/vsftpd.conf
#connect_from_port_20=YES
anonymous_enable=NO
local_enable=YES
#read only
write_enable=YES
#enable logging of transfers
#xferlog_std_format=YES

#idle_session_timeout=20
#data_connection_timeout=20
#nopriv_user=nobody

#chroot_list_enable=YES
#chroot_list_file=/etc/vsftpd/chrootlist

#ls_recurse_enable=NO
```

## Open `vsftpd` service
```bash
rc-server vsftpd start
## or
/etc/init.d/vsftpd start
```
