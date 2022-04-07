---
layout:     post
title:      "14th week group meeting record"
subtitle:   "normal speed"
date:       2022-04-07 21:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - 组会记录
    - PSC code
    - PWSO model
    - baldur
    - discussion
---

## 第十四周组会记录
## 1. 平衡

### 	修改文章

### 	PWSO 0D 模型

由PWSO 0D 模型推出的密度极限与 Greenwald 密度极限的对比

   PWSO 0D 模型推出的密度极限

$$
n_{c}=\frac{2 D_{\perp}}{f \lambda \operatorname{Rad}[T(r)]} \frac{T_{t}}{I\left(T_{t}\right) a} \propto \frac{T_{t}}{I\left(T_{t}\right) a}
$$


垂直扩散系数：

- **Bohm diffusion** [stangeby 2000 Eq. (4.8); D. Bohm, in  *The characteristics of electrical discharges in magnetic fields* Eq. (8)]
  $q\approx \frac{aB_\phi}{RB_p}, 2\pi aB_p=\mu_0I_0$, 
  $$
  D_\perp\propto Te/B\propto\frac{a^2T}{qRI_0}
  $$
  
  $$
  n_{c}=\frac{2 D_{\perp}}{f \lambda \operatorname{Rad}[T(r)]} \frac{T_{t}}{I\left(T_{t}\right) a} \propto \frac{T_{t}}{I\left(T_{t}\right)}\frac{aT}{qRI_0f\lambda \text{Rad}}
  $$
  
- **Classical diffusion** [stangeby 2000 Eq. (4.9); Goldston R. J. *Introduction to plasma physics* 1997, p196]
  $\rho_\|^{\text{spitzer}}\approx 8\times 10^{-4}/T_e^{3/2}, D_\perp^{\text{classical}}=2\rho_\|^{\text{spitzer}}nk(T_e+T_i)/B^2\propto nT/B^2$

- **Neoclassical diffusion**



中性粒子电离位置 $\lambda$ 与输运系数、温度有关，需要找到相应模型进行描述。

## 2. 辐射

将目前的 ignitor 算例对照着 baldur 文献中程序结构图运行了一遍。列了一个大致的程序修改计划。

## 3. GPU

PSC with CPU only 在 mhd, hust, 北京并行超算上都可以成功编译、运行。

PSC with CUDA 在 mhd, hust, 北京并行超算上的编译有各种没解决的问题。

## 下周计划

- 修改文章
- 继续查找根据 PWSO 0D 模型推导出的 $n_c$ 中 $D_\perp,~\lambda$ 与磁场、电流、温度的关系，从而与 Greenwald 密度极限对比。
  可以查找 EMC3 程序文献 [Y. Feng 1999] 中的扩散系数做参考。
- 开始修改 baldur 程序，以 PPT 形式讲 baldur 参考文献。
- 学习 OpenACC；安装 heq 系统。 



## Program BALDUR

按照如下流程，可以将程序大致分为 30 步，大致一周一步

#### Outer calling structure

1. Set up I/O units (except for ripple input data file). 
2. Call **BLOCKDTA** 
   Store fundamental constants. 
3. Call **MASTER**
   1. Call **BASIC** 
      Initialize OLYMPUS variables. Call **MODIFY**, which reads input deck header cord used for restart switches (not implemented in present revision of **BALDUR**). 
   2. Print date and time.
   3. Call **COTROL** 
      1. Call **LABRUN** 
         Store **BALDUR** version number. Read runs labels from input deck. Work revision number, labels, “PROGRAM BALDUR”. 
      2. Call **CLEAR** 
         Clear common blocks. Includes calls to **ACLEAR**, **GCLEAR**, **HCLEAR**, **ZCLEAR**. 
      3. If run is not a restart 
         Call **PRESET** 
         Set default values for NAMELIST input variables ($\leftarrow$ appendix D). 
      4. If run is a restart 
         Call **RESUME** 
         Restart capability is not implemented in present version of **BALDUR**. Do not use this branch. 
      5. Call **DATA** 
         Read and write *NAMELIST* input. 
      6. Call **AUXVAL** 
         1. Call **ERRCHK**
            Check for obvious mistakes in input deck. 
         2. Call **UNITS** 
            Computer units conversion factors. 
         3. Set auxiliary values based on input data. This includes code flags, the radial grid, time dependent conditions, plasma conditions variables not defined in INITIAL or START, etc. ($\leftarrow$ section 4). 
         4. If scrape-off model is on, output sheath limited current densities ($\leftarrow$ section 2.8). 
         5. <font color=red> Call **IMPRAD(1)**</font>
            1. Initialize neutral impurity influx parameters ($\leftarrow$ section 2.9.13). 
            2. If scrape-off model is on 
               Call **PDX** 
               Initialize and compute scrape-off losses ($\leftarrow$ section 2.9.5). 
            3. Initialize coronal radiation package. 
               Read atomic data file `FOR22` ($\leftarrow$ section 7.3). 
         6.  <font color=red> Call **BEAMS(1)** </font>
            Initialize neutral beam package ($\leftarrow$ section 2.9.2). 
         7. If run is not a restart 
            Call **INITAL** 
            1. <font color=red> Call **IMPRAD(2)**</font>
               1. If scrapeoff model is on 
                  Call **PDX** 
                  Initialize and compute scrapeoff losses ($\leftarrow$ section 2.9.5). 
               2. Compute initial radiation terms, impurity charge states. Call coronal radiation package if selected ($\leftarrow$ section 2.9.4).  
            2. Define physical initial conditions for $\chi$ ($\leftarrow$ section 5.2.3), $B_\theta$.
               Note that the electron density is redefined in START (~section 4).
         8. Call **START**
            1. Set initial $\Delta t$, boundary conditions, volume integrals. Unlock neutral impurity influx flag. 
            2. Call **GETCHI(2)** 
               1. <font color=red> Call **IMPRAD(2)** </font>
                  Same as above, plus compute neutral impurity and He influxes. Includes call to PDX. 
               2. Compute boundary-centered densities and temperatures, zone-centered $B_\theta$ and $J$ ($\leftarrow$ section 5.2.2). 
                  Compute Z-effective, electron density. 
               3. <font color=red> Initialize packages for remaining source terms: call **HEAT(1)** (lower hybrid and input profile heating), **ECRH(1)** (electron cyclotron resonance heating), and either **ALFINI(alphas)** or **HE3(1)** (D-D fusion) as selected by input.</font>  
         9. Call **OUTPUT(1)** 
            Initial alphanumeric output (call **MPRINT**, **GPRINT**, **HPRINT**, **IPRINT**, **APRINT **or **FPRINT**, **RPRINT**, **SPRINT**). Initial graphics output (call **TGRAF(1)**, **GRAFIX(1)**). 
         10. Call **STEPON** 
             Do a time step of main computations. See ext. chart. 
         11. Call **OUTPUT(2)** 
             Control output flags. At times or time steps selected by input, do main alphanumeric output (call **MPRINT**, **NCFPRT**, **GPRINT**, **HETPRT**, **HPRINT**, **IPRINT** and either **APRINT**, **FPRINT**, as selected by input), short alphanumeric output (call **SPRING**, **GPRINT**), and/or graphics output (call **TGRAF(2)**, **GRAFIX(2)**). 
         12. CALL **TSEND** Test for completion of run. 
         13. If run is not yet completed, do next time step: go to \#10. 
         14. Call **OUTPUT(3)** 
             Do main alphanumeric output for final time step if this has not been done already (same calls as in \#11). Do final graphics output and close graphics (call **TGRAF(3)**, **GRAFIX(3)**. 
         15. Call **ENDRUN** 
             Terminate run. Print final message, STOP.

#### Inner calling structure

10. Call **STEPON**
    1. Do short teletype output at appropriate time steps. Zero the iteration counter. 
       CALL **SAWMIX** 
    2. Call **RESOLV(1)** 
       Initialize flags and store values for predictor-corrector, extrapolation. 
    3. Unlock neutral impurity influx flag. 
    4. Call **COEF**
       1. Call **GETCHI(2)**
          1. <font color=red> Call **IMPRAD(2)**</font>
             1. If scrapeoff model is on, 
                Call **PDX** 
                Compute scrapeoff losses 
                	Call **DIVER**. 
             2. Compute radiation terms, impurity $\left<Z\right>$ and $\left<Z^2\right>$, calling coronal radiation package if selected by input. 
             3. Compute neutral impurity and its influxes. 
          2. Compute boundary-centered densities and temperatures, zone-centered $B_\theta$ and $J$ ($\leftarrow$ section 5.2.2). Compute $Z_{\text{eff}}$, electron density.
       2. Call **TRCOEF** 
          Compute transport coefficients. This may include calls to **EMPIRC**, **XSCALE**, **INIGRL**. On initial call if ripple input data file is being used, open and read this file and call **AVERAGE**. 
       3. If this is a repeated time step, or the corrector portion of a predictor-corrector time step, or a $\Delta t/2$ minor time step of an extrapolation time step, skip to \#10. 
       4. <font color=red> Call **NEUGAS** </font>
          Compute sources due to neutral gas. This includes calls to PDX if scrapeoff model is on, and to the Monte Carlo neutral gas package at appropriate time steps. Compute neutral hydrogen influxes, either by prescription or by density monitor. 
       5. <font color=red> Call **BEAMS(2)** </font>
          Compute sources due to neutral beam injection. This includes Fokker-Planck calculation of beam fast ion distribution, and every so often includes call to Monte Carlo computation of beam deposition profile. 
       6. <font color=red> Call **HEAT(2)** </font>
          Compute sources due to lower hybrid heating or input profile heating. 
       7. <font color=red> Call **ECRH(1)** </font>
          Compute sources due to electron cyclotron resonance heating. 
       8. <font color=red> If D-T fusion has been selected by input,</font>
          Call **ALPHAS(2)** 
          Compute sources due to D-T fusion. This includes calculation of alpha-particle distribution, and includes call to Monte Carlo computation of alpha-particle orbits. 
       9. <font color=red> If D-D fusion has been selected by input, </font>
          Call **HE3(2)** 
          Compute sources due to D-T fusion. 
       10. Call **CONVRT** 
           1. If nearly exact neoclassical transport model has been selected, compute transport coefficients for this model. This includes call to **NCFLUX**. 
           2. Compute **A**, **B**, **C**, **D** ($\leftarrow$ appendix A). 
       11. Call **CNVCOF** 
           Convert **A**, **B**, **C**, **D** ($\leftarrow$ appendix A) from standard to interval units. 
    5. Call **SOLVEB** 
       Solve $B_\theta$ equation. Compute source due to ohmic heating. 
    6. Call **BOUNDS** 
       Compute boundary condition coefficients.
    7. Call **REDUCE** 
       Compute **P**, **Q**, **R**, **S** ($\leftarrow$ eq. 5.2.3g). Reduce equations to first order. 
    8. Call **SOLVE** Solve transport equations for new $\chi$
    9. Call **RESOLV** 
       Time-step control. Set next $\Delta t$. Control flags and store values for predictor-corrector. extrapolation, time step repetition. 
    10. Increment iteration counter. Check that maximum number of iterations has not been exceeded. 
    11. If time step is to be repeated, or the predictor portion of a predictor-corrector time step has just finished, or the first $\Delta t/2$ minor time step of an extrapolation time step has just finished, go to #4. 
    12. If the $\Delta t$ minor time step of an extrapolation time step has just finished, go to #5. 
    13. Call **CMPRES** 
        1. Compress the plasma: adjust $\chi$, $B_\theta$ radial grid. 
        2. Call other routines which may need to compress: **BEAMS(3)**, HEAT(3) and either **ALPHAS(3)** or **HE3(3)** as selected by input. 
    14. <font color=red> Call **PDRIVE** Compute sources due to pellet injection. </font>
    15. Call **GETCHI(1)** 
        Update quantities based on $\chi$: densities in standard units, ion and electron temperatures. Also update $B_\theta$ $B_Z$ in standard units. Note that electron density (but not electron temperature) is recomputed in **GETCHI(2)**. 
    16. <font color=red> Lock neutral impurity influx flag</font>. 
    17. <font color=red> Call **GETCHI(2)** </font>
        Almost same as **COEF#1**, but new $\chi$ values were used, and there is no neutral impurity or He influx computation when **IMPRAD(2)** is called. 
    18. Advance the time. Time step is completed.
