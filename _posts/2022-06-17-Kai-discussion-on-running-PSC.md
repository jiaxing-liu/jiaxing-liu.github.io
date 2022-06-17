---
layout:     post
title:      "Discussion of running PSC using PBS"
subtitle:   "usage of OpenMPI"
date:       2022-06-17 15:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - discussion
    - MPI
    - PSC
    - PBS
    - slurm
---

# 2022-06-17 讨论 PSC

## can not run `psc_harris_xz` case with PBS on multi-nodes

### 大集群：
#### 1. 查看 openmpi 版本安装信息 `ompi_info` 

系统 openmpi 版本信息

```bash
[jxliu@ln01 src]$ /usr/mpi/gcc/openmpi-4.0.3rc4/bin/ompi_info |grep ess
          Thread support: posix (MPI_THREAD_MULTIPLE: yes, OPAL support: yes, OMPI progress: no, ORTE progress: yes, Event lib: yes)
            MCA compress: gzip (MCA v2.1.0, API v2.0.0, Component v4.0.3)
            MCA compress: bzip (MCA v2.1.0, API v2.0.0, Component v4.0.3)
                 MCA ess: hnp (MCA v2.1.0, API v3.0.0, Component v4.0.3)
                 MCA ess: singleton (MCA v2.1.0, API v3.0.0, Component v4.0.3)
                 MCA ess: slurm (MCA v2.1.0, API v3.0.0, Component v4.0.3)
                 MCA ess: pmi (MCA v2.1.0, API v3.0.0, Component v4.0.3)
                 MCA ess: tool (MCA v2.1.0, API v3.0.0, Component v4.0.3)
                 MCA ess: env (MCA v2.1.0, API v3.0.0, Component v4.0.3)
           MCA vprotocol: pessimist (MCA v2.1.0, API v2.0.0, Component v4.0.3)
```

可以看到这个路径下 `openmpi` 依赖的作业提交系统为 `slurm`，而系统安装的是 `PBS(Torque)`，这可能是导致不能多节点运行的原因。

spack openmpi 安装信息
```bash
[jxliu@ln01 src]$ ~/software/spack/opt/spack/linux-centos7-skylake_avx512/gcc-11.2.0/openmpi-4.1.3-asxpzo6rfmdantyimad7mxjy2ftligqr/bin/ompi_info |grep ess
          Thread support: posix (MPI_THREAD_MULTIPLE: yes, OPAL support: yes, OMPI progress: no, ORTE progress: yes, Event lib: yes)
            MCA compress: bzip (MCA v2.1.0, API v2.0.0, Component v4.1.3)
            MCA compress: gzip (MCA v2.1.0, API v2.0.0, Component v4.1.3)
                 MCA ess: env (MCA v2.1.0, API v3.0.0, Component v4.1.3)
                 MCA ess: hnp (MCA v2.1.0, API v3.0.0, Component v4.1.3)
                 MCA ess: pmi (MCA v2.1.0, API v3.0.0, Component v4.1.3)
                 MCA ess: singleton (MCA v2.1.0, API v3.0.0, Component v4.1.3)
                 MCA ess: tool (MCA v2.1.0, API v3.0.0, Component v4.1.3)
                 MCA ess: tm (MCA v2.1.0, API v3.0.0, Component v4.1.3)
           MCA vprotocol: pessimist (MCA v2.1.0, API v2.0.0, Component v4.1.3)
```

可以看到，这个版本依赖的作业提交系统为 `tm(torque)`。

但是使用该版本 `openmpi` 运行 `psc_harris_xz`，仍然有如下报错信息

```bash
[cu45][[58384,1],32][btl_tcp_endpoint.c:625:mca_btl_tcp_endpoint_recv_connect_ack] received unexpected process identifier [[58384,1],47]
```

```bash
--------------------------------------------------------------------------
WARNING: Open MPI accepted a TCP connection from what appears to be a
another Open MPI process but cannot find a corresponding process
entry for that peer.

This attempted connection will be ignored; your MPI job may or may not
continue properly.

  Local host: cu47
  PID:        179815
--------------------------------------------------------------------------
```

####  2. `ldd` 查看依赖库文件

使用 `ldd psc_harris-xz` 进一步发现 `hdf5` 和 `adios2` 与 使用的 mpi 不是同一版本 (scheduler=none)

使用 `spack` 重新编译安装依赖于 openmpi(scheduler=tm) 的 `adios2` 和 `hdf5`，并重新编译 `psc`

####  3. 限制 `openmpi` 使用的端口

之后使用交互式作业方式进行调试，发现仍有相同报错。在 [这里](https://blog.csdn.net/weixin_43985478/article/details/118361625) 找到了类似的解决方案，加上 flag `--mca btl_tcp_if_include ib0` 来限制 mpi 使用的端口，可以多节点运行 `psc`。

batch 文件如下：
```bash
#!/bin/bash
#PBS -N harris-128
#PBS -q regular
#PBS -l nodes=2:ppn=32
####PBS -l nodes=cu01:ppn=48+cu02:ppn=48+cu03:ppn=32
#PBS -l walltime=2:00:00:00
cd $PBS_O_WORKDIR
set -x
. /home/jxliu/software/spack/share/spack/setup-env.sh
spack load adios2@2.8.0/di25oxj
spack load cmake@3.23.1
spack load hdf5@1.12.2/ivnns3f
spack load openmpi/asxpzo6

mpirun --mca btl_tcp_if_include ib0 /home/jxliu/space-plasma/psc/src/psc-openmpi@4.1.3-adios2-tm/build/src/psc_harris_xz > psc_harris_xz.log 2>psc_harris_xz-war.log
```
#### 其他

提交交互式作业调试

``` bash
qsub -I -l nodes=2:ppn=32
```
