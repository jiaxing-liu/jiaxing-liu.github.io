---
layout:     post
title:      "Install Slurm on Gentoo OS"
subtitle:   "High Performance Computing"
date:       2022-05-19 1:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - LINUX
    - HPC
    - slurm
    - Gentoo
---

## Gentoo slurm 安装

### 安装 slurm

在所有节点使用 emerge 安装 sys-cluster/slurm，emerge 将自动安装 munge 并创建固定 uid, gid 的 munge, slurm 用户

```bash
emerge -av sys-cluster/slurm
```

### 配置测试 munged

开启所有节点 munged 服务并设置为开机启动，将管理节点 `/etc/munge/munge.key` 复制到其他节点相应位置  `/etc/munge` ，

```bash
rc-service munged start
rc-update add munged default
rsync -auvz --delete /etc/munge/munge.key io01:/etc/munge
```

测试 munged 是否配置成功，在管理节点运行一下命令

```bash
munge -n |unmunge  #测试管理节点
munge -n |ssh cu201 unmunge  #测试其他(cu201)节点
```

应得到类似如下输出

```bash
STATUS:           Success (0)
ENCODE_HOST:      cu204 (127.0.0.1)
ENCODE_TIME:      2022-05-19 00:54:32 +0800 (1652892872)
DECODE_TIME:      2022-05-19 00:54:32 +0800 (1652892872)
TTL:              300
CIPHER:           aes128 (4)
MAC:              sha256 (5)
ZIP:              none (0)
UID:              root (0)
GID:              root (0)
LENGTH:           0
```

### 配置 slurm.conf

根据软件提供网页工具 `/usr/share/doc/slurm-20.11.0.1-r104/html/configurator.html` 进行配置，按照实际情况填写并生成 `slurm.conf`，复制到 `/etc/slurm/slurm.conf`，可与提供的`/etc/slurm/slurm.conf.example` 对照填写。

若 `slurm.conf` 中 ProctrackType=proctrack/cgroup，则需要创建 `/etc/slurm/cgroup.conf` ,使用默认的即可。

遇到如下错误

```bash
heq /var/log/slurm # sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
regular*     up   infinite      4  drain cu[201-204]
```

进一步查看得到

```bash
heq /var/log/slurm # sinfo -R
REASON               USER      TIMESTAMP           NODELIST
Low RealMemory       root      2022-05-18T17:20:20 cu201
Low RealMemory       root      2022-05-18T17:22:33 cu202
Low RealMemory       root      2022-05-18T17:22:44 cu203
Low RealMemory       root      2022-05-18T17:22:54 cu204
```

```bash
heq /var/log/slurm # scontrol show node
NodeName=cu201 Arch=x86_64 CoresPerSocket=24
   CPUAlloc=0 CPUTot=48 CPULoad=0.00
   AvailableFeatures=(null)
   ActiveFeatures=(null)
   Gres=(null)
   NodeAddr=cu201 NodeHostName=cu201 Version=20.11.0
   OS=Linux 5.15.32-gentoo-r1-x86_64 #1 SMP Tue May 10 16:55:58 CST 2022
   RealMemory=186000 AllocMem=0 FreeMem=186649 Sockets=2 Boards=1
   State=IDLE+DRAIN ThreadsPerCore=1 TmpDisk=0 Weight=1 Owner=N/A MCS_label=N/A
   Partitions=regular
   BootTime=2022-05-10T20:04:13 SlurmdStartTime=2022-05-18T19:48:08
   CfgTRES=cpu=48,mem=186000M,billing=48
   AllocTRES=
   CapWatts=n/a
   CurrentWatts=0 AveWatts=0
   ExtSensorsJoules=n/s ExtSensorsWatts=0 ExtSensorsTemp=n/s
   Reason=Low RealMemory [root@2022-05-18T17:20:20]
   Comment=(null)
```

错误原因是因为 `slurm.conf` 中设置的 RealMemory 大于 计算几点实际可用内存，减小 `slurm.conf` 中RealMemory 的值并使其生效即可。使其生效需运行如下命令：

```bash
scontrol reconfigure
## 更新计算节点状态
scontrol update nodename=cu201 state=resume
```



参考链接

[]: https://southgreenplatform.github.io/trainings/hpc/slurminstallation/#part-1	"SouthGreen"
[]: https://slurm.schedmd.com/quickstart_admin.html	"slurm 官方教程"
[]: https://slurm-dev.schedmd.narkive.com/lnoGic15/unknown	"slurm dev"

[]: https://stackoverflow.com/questions/68132982/slurm-says-drained-low-realmemory	"stackoverflow"

[slurm 官方教程](https://slurm.schedmd.com/quickstart_admin.html)

