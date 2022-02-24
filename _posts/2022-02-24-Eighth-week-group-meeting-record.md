---
layout:     post
title:      "the use of VNC in linux"
subtitle:   "in Gentoo linux"
date:       2022-02-24 21:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
tags:
    - 组会记录
    - PSC code
    - PWSO model
    - Green function
---

# 第八周

# 0. 上周计划

![image-20220224194425893](C:\Users\FIRSTROG\AppData\Roaming\Typora\typora-user-images\image-20220224194425893.png)

# 1. PSC

cmake.sh 文件如下

```bash
spack load cuda@11.4.1
spack load adios2@2.7.1
spack load openmpi@4.1.2
spack load thrust@1.12.0
export ADIOS2_DIR=/data/data1/software/PSC/spack/opt/spack/linux-gentoo2-skylake/gcc-10.3.0/adios2-2.7.1-jaxcfloyxp4ko27z2m2o6eg5gbbv5jgn
cmake -S ../ -DUSE_CUDA=on
```

nvidia-drivers 470和相应版本的cuda11.4

PSC程序运行结果和mhd之前结果相同

- CPU only : flatfoil, harris(with 4 cores)可以运行
- CPU+GPU: flatfoil卡在第一步，harris可以运行但是不调用GPU

# 2. PWSO
- MARFE formation

  ![image-20220224162736512](C:\Users\FIRSTROG\AppData\Roaming\Typora\typora-user-images\image-20220224162736512.png)

  功率平衡：
  $$
  \begin{equation}
  \frac{4}{7} \kappa_{0}\left(T_{0}^{7 / 2}-T_{m}^{7 / 2}\right) / L=\alpha_{z} n_{m}^{2} R_{z}\left(T_{m}\right) L_{m}
  \end{equation}
  $$
  ​	其中，右侧为杂质辐射功率，左侧为MARFE区域输入功率，由以下公式导出[The Plasma Boundary of Magnetic Fusion Devices, Stangeby, 2000]
  $$
  \begin{array}{rl}
  \frac{d}{d s_{\|}} \left(q_{\| \text {cond }}\right)=&\frac{\left(P_{\text {SOL }} / A_{q \|}\right)}{L}=-\kappa_{0} T^{5 / 2} \frac{\mathrm{d}^2 T}{ds_\|^2}\\
  T\left(s_{\|}\right)=&\left[T_{u}^{7 / 2}-\frac{7}{4} \frac{\left(P_{\mathrm{SOL}} / A_{q \|}\right)}{L} \frac{s_{\|}^{2}}{\kappa_{0}}\right]^{2 / 7}\\
  T\left(s_{\|}\right)=&\left[T_{t}^{7 / 2}+\frac{7}{4} \frac{\left(P_{\mathrm{SOL}} / A_{q \|}\right)}{L} \frac{\left(s_{\|}-L\right)^{2}}{\kappa_{0}}\right]^{2 / 7}\\
  P_{\text{SOL}}=&\frac{4}{7}\frac{T_u^\frac{2}{7}\kappa_0A_{q\|}}{L}
  \end{array}\\
  $$


  假设压强守恒：
  $$
  n_0T_0=n_mT_m
  $$
  上式可以给出
  $$
  \frac{L_{z}\left(T_{m}\right)}{T_{m}^{2}}=\frac{4 \kappa_{0}}{7 \alpha_{z} f_{m}} \frac{T_{0}^{3 / 2}}{L^{2} n_{0}^{2}}
  $$
  使用 $L=\pi qR$，和q的定义得到：
  $$
  n_{\mathrm{c}}=\frac{I_{\mathrm{P}}}{\pi a^{2}} \frac{1}{B_{\mathrm{T}}}\left[\frac{\mu_{0}^{2} \kappa_{0} T_{0}^{3 / 2}}{7 \pi^{2} \alpha_{\mathrm{Z}} f_{\mathrm{m}}\left\{R_{\mathrm{Z}} / T^{2}\right\}_{\max }}\right]^{1 / 2}
  $$
  依赖参数为 $I_p$ 和 $a$

- PWSO 0D model 
  $$
  n_{c}=\frac{2 D_{\perp}}{f \lambda \operatorname{Rad}[T(r)]} \frac{T_{t}}{I\left(T_{t}\right) a} \propto \frac{T_{t}}{I\left(T_{t}\right) a}
  $$
  依赖参数为温度 $T_t, a$ 和溅射函数 $I\left(T_t\right)$ 

以上两个理论得到的密度极限所依赖参数不一样，且不能联系在一起，因此两个理论不具备可比性。

- 达到 density limit 需要的初始(指 tokamak 启动刚刚完成)密度

  假设边界(靶板)温度为 30eV 左右，初始密度$10^{20}m^{-3}$ 

# 3. Green function奇异性

$$
G\left(\mathbf{R} ; \mathbf{R}^{\prime}\right)=\frac{1}{2 \pi} \frac{\sqrt{R R^{\prime}}}{k}\left[\left(2-k^{2}\right) K(k)-2 E(k)\right]
$$

其中 
$$
k^{2}=\frac{4 R R^{\prime}}{\left[\left(R+R^{\prime}\right)^{2}+\left(Z-Z^{\prime}\right)^{2}\right]}
$$
单位环向电流密度产生磁通：
$$
\Psi_{p}\left(R^{\prime}, Z^{\prime}\right)=\int_{\mathrm{Plasma}} G\left(R, Z ; R^{\prime}, Z^{\prime}\right) d R d Z
$$
这里，$R,R^{\prime}>0$，所以$\color{red}{格林函数没有奇异性}$

$\eta=0$时，LCFS处相对误差；横轴为$\theta$，纵轴为相对误差![image-20220224195807689](C:\Users\FIRSTROG\AppData\Roaming\Typora\typora-user-images\image-20220224195807689.png)

# 4. 下周计划：

- 考虑 PWSO 1D 模型是否可以推出 Greenwald density limit
- 考虑真空解 X 点处“奇异性”来源；计算由格林函数求得的真空解误差。
- VDE 理论学习

