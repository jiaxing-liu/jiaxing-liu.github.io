---
layout:     post
title:      "Compact WSL"
subtitle:   "WSL"
date:       2025-06-25 21:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
tags:
    - WSL
---



##  压缩 WSL 磁盘的方法

1. **关闭所有 WSL 实例：**

   ```bash
   wsl --shutdown
   ```

2. **压缩 ext4.vhdx**
    打开 PowerShell（管理员）运行：

   ```powershell
   diskpart
   ```

   然后在 diskpart 提示符中运行：

   ```text
   select vdisk file="C:\Users\<你的用户名>\AppData\Local\Packages\"C:\Users\FIRSTROG\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu20.04onWindows_79rhkp1fndgsc\LocalState\ext4.vhdx"
   attach vdisk readonly
   compact vdisk
   detach vdisk
   exit
   ```

   ## Powershell 中设置防火墙使 WSL 可以使用 windows 代理

   ```bash
    New-NetFirewallRule -DisplayName "Allow WSL Access to v2rayA HTTP Proxy" `
>>   -Direction Inbound `
>>   -Action Allow `
>>   -Protocol TCP `
>>   -LocalPort 20171 `
>>   -RemoteAddress 172.29.16.0/20 `
>>   -Profile Any
   ```
