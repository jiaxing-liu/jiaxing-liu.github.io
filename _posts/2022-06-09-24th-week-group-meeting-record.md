---
layout:     post
title:      "24th week group meeting record"
subtitle:   "PWSO-1D PSC"
date:       2022-06-11 16:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - 组会记录
    - PWSO model
    - PSC
    - discussion
---

# 第 24 周组会记录


## 1. 辐射

#### PWSO 1D code

$$
\begin{array}{rl}
\partial_{t} n_{i}-D \partial_{x}^{2} n_{i}=&C_{i}\left[\partial_{x} T\left(r_{\text {LCFS }}, t-\tau_{\text {delay }}\right)+T_{\text {loss }}^{\prime}\right] \delta(x-a+\lambda)\\
&\\
n \partial_{t} T-K \partial_{x}^{2} T=&C_T T^{2/3}+p_{\text {add }}-n n_{i} \operatorname{Rad}(T)
\end{array}
$$

边界条件

- 杂质粒子输运方程
  $n_i(0)=0, \frac{\partial n}{\partial x}|_{x=a}=0$
- 热输运方程
  $T(a)=T_t, \frac{\partial T}{\partial x}|_{x=0}=0$


考虑聚变反应的发生，热输运方程中的加热项($P_\text{add}$)、辐射项($-n n_{i} \operatorname{Rad}(T)$) 可以具化为 $\alpha$ 粒子加热和回旋辐射、线辐射等。预计可以得到一个新的密度极限依赖关系和总辐射($R$)随时间的演化。这里 $R$ 的定义如下：
$$
	R=P-P_{\mathrm{LCFS}}(t)+R_{\mathrm{SOL}}
$$

其中，

$$
\begin{array}{rl}
		P&=a^{\prime} L \int_{0}^{r_{\mathrm{LCFS}}}\left[C_{T} T(x)^{3 / 2}+p_{\mathrm{add}}(x)\right] \mathrm{d} x,\\
		&\\
		&P_{\mathrm{LCFS}}=-a^{\prime} L K T^{\prime}\left(r_{\mathrm{LCFS}}\right)
		\end{array}
$$


有 $\alpha$ 粒子加热 (PATH: `mhd: /data/data12/density_limit/pwso_Escande/1d/jxliu-burning/scan-n0-increase_padd`)
 <img src="D:\Grad\study\Groupmeeting\2022\figure\24thweek\ptpx-R-alpha.png" style="zoom:80%;" />
$P_{\text{add}}$ 为常数 (PATH: `mhd:/data/data12/density_limit/pwso_Escande/1d/Esconde/scan-n0-increase_padd`)
<img src="D:\Grad\study\Groupmeeting\2022\figure\24thweek\ptpx-R-escande.png" style="zoom:80%;" />


## 2. PSC

#### CPU 加速效果

不同核数下运行 harris 算例的时间 (PATH: `mhd:/home/jxliu/data2/space-plasma/psc/cases/scan-np`)

<img src="D:\Grad\study\Groupmeeting\2022\figure\24thweek\time.png" style="zoom:80%;" />

使用虚拟化技术运用更多核数计算时，如 128 核，并不能减小时间。

#### 粒子丢失问题

使用不同核数，计算结果有差别，是因为计算过程中，某些 domain 内粒子数过多、内存分配问题。
   可以改变 `nppc` (number of macro particle per cell) 和 `overalloc` (overallocation factor for particle arrays) 参数\\
尝试解决。

- 增大 `nppc` 可以增加模拟的总粒子数，减小粒子丢失带来的误差
- 增大 `overalloc` 增加一个 domain 可以容忍的粒子数，减小粒子丢失

![output](output.gif)



#### 使用 PBS 提交 算例问题

##### 大集群

错误信息

```bash
There are not enough slots available in the system to satisfy the 128
slots that were requested by the application:

  /home/jxliu/space-plasma/psc/src/scan-np/psc-np128/build/src/psc_harris_xz
```

尝试解决方案：

1. 使用 spack 重新安装 `openmpi schedulers=tm` ,  仍然报错
2. 使用系统 openmpi `/opt/mpi/gcc/openmpi-4.0.3rc4` , 仍然报错

##### heq cluster
和大集群一样的情况

## 下周计划

1. 文章回复
2. PWSO 1D code
3. 修改 baldur
4. cylmhd 算例
