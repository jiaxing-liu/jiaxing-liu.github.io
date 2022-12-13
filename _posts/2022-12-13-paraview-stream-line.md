---
layout:     post
title:      "Plot stream line using Paraview"
subtitle:   "txt file"
date:       2022-11-20 13:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Paraview
    - visualization
    - stream line
---

## Plot stream line using Paraview

### Load txt file
1. 不勾选 "Have headers"
2. Field Delimiter Characters 填入 `" "`
3. Apply

### 添加 `Filter:TableToPoints2`
1. 选择相应的 `X,Y,Z` 轴

这里选择二维数据，勾选上 `2D Points`
2. Apply

### 添加 `Filter:PointVolumeInterpolator`

Apply

### 添加 `Filter:Calculator`

计算出要追踪的矢量

Result Array Name `B2D`
`"Field 2"*iHat+"Field 3"*jHat`

Apply.

### 添加 `Field:StreamTracer`
1. 选择要追踪的矢量
2. 选择积分起始位置：Seed type
3. Apply