---
layout:     post
title:      "No password authercation after replacing motherboard"
subtitle:   "windows"
date:       2023-04-19 12:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Windows
    - 注册表
---

## 更换主板后，有如下问题：没有密码登录的选项；PIN 码和面部识别无法登录。

<center>
  <img src="http://47.105.42.5:8939/jxliu/images/-/raw/master/pictures/2023/04/19_19_5_14_problem_win.jpg" width=80%>
</center>

## 解决方法：修改注册表以显示密码登录的选项。
```bash
计算机\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PasswordLess\Device\DevicePasswordLessBuildVersion=0
```
