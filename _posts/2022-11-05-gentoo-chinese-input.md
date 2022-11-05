---
layout:     post
title:      "Chinese input method in Gentoo OS"
subtitle:   "ibus"
date:       2022-11-05 13:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - input method
    - ibus
---

## Compile ibus and dependencies

```bash
root #emerge --ask app-i18n/ibus ibus-libpinyin
```

Add use flag to `/etc/portage/package.use/ibus`
```bash
dev-qt/qtgui ibus
kde-plasma/plasma-desktop ibus
```


```bash
root #emerge --ask --oneshot --newuse dev-qt/qtgui kde-plasma/plasma-desktop
```

Uninstall `fcitx-xxx` software

## Add environments and reboot

Add to `/etc/environment`
```bash
GTK_IM_MODULE=ibus
QT_IM_MODULE=ibus
XMODIFIERS=@im=ibus
```

Using command `ibus-setup` to start ibus daemon
