---
layout:     post
title:      "21th week group meeting record"
subtitle:   "slow speed"
date:       2022-05-20 16:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - 组会记录
    - PWSO model
    - PSC
    - discussion
---

# 第 21 周组会记录
## 上周计划

1. PWSO 模型
2. 规范 baldur 程序，学习 baldur 文章 [C.E. SINGER et al 1988]
3. 学习 PSC harris 算例

## 1. 平衡

### 	PWSO 0D 模型

J-TEXT 上密度极限
$$
n_{c}=\frac{2 D_{\perp}}{f \lambda \operatorname{Rad}[T(r)]} \frac{T_{t}}{I\left(T_{t}\right) a}
$$


$D_\perp=1.0 m^2s^{-1}, f=1.0, \lambda=0.01m, \text{Rad}=10^{-32} W m^3,  a=0.25 m$

$I(T_t)$ 包括物理溅射、化学溅射（常数）。靶板材料：碳


![denlim-Tt-che](denlim-Tt-che.png)

## 2. 辐射

## 3. PSC

1. harris current sheet



## 下周计划
1. PWSO

   - 详细记录如何求得 J-TEXT 上密度极限。(包括 2-point model, 化学溅射，参数选取依据)
   - 数值上与 Greenwald 密度极限做对比
2. 辐射
   - 规范 baldur 程序，学习 baldur 文章 [C.E. SINGER et al 1988]
3. 运行 harris 算例
   - 关注 $m_i/m_e$，计算区域大小，CPU 核数，计算时间，结果是否物理。

## PWSO 方向

1. 论证 PWSO 模型合理性
   - PWSO 密度极限相关参数在合理变化范围内是否可以得到Greenwald密度极限(传统和依赖于P)
   - 是否可以在选取一定模型后（或者在相关参数区间内）得到与Greenwald 密度极限趋势一致的结果
2. 使用 PWSO 模型预测新的密度极限
   - 在什么模型/参数条件下，该模型预测出密度极限符合 Greenwald 密度极限。
   - 在什么模型/参数条件下，该模型可以预测出新的密度极限 

3. 推广 PWSO 模型
   - 将 PWSO 推广至燃烧等离子体区域。（功率 P、辐射R 受聚变反应影响）
