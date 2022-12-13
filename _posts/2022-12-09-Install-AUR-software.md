---
layout:     post
title:      "Install AUR software"
subtitle:   "manjaro/arch"
date:       2022-11-20 13:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - manjaro
    - arch
    - AUR
---

## Install AUR software

### Using `yay`

### Using `git` and `pacman`
reference:[archlinux wiki](https://wiki.archlinux.org/title/Arch_User_Repository#Installing_and_upgrading_packages)

```bash
cd ~/build
git clone https://aur.archlinux.org/package_name.git
cd package_name
makepkg
pacman -U package_name-version-architecture.pkg.tar.zst
```
