---
layout:     post
title:      "Roche potential"
subtitle:   "Binary Stars"
date:       2022-12-13 22:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Roche potential
    - Binary starts
    - space-plasma
    - notes
---

## Roche potential

$$
\Phi_R(\vec{r})=-\frac{G M_1 M_{\odot}}{\left|\vec{r}-\vec{r}_1\right|}-\frac{G M_2 M_{\odot}}{\left|\vec{r}-\vec{r}_2\right|}-\frac{1}{2}(\vec{\omega} \times \vec{r})^2
$$

$$
\vec{\omega}=\left[\frac{G\left(M_1+M_2\right) M_{\odot}}{a^3}\right]^{1 / 2} \vec{e}
$$

```gnuplot
set term png
unset surface
set contour
set view map
set hidden3d
unset key
set xlabel 'R'
set ylabel 'Z'
set label 1 'L_1' at 0.3,-0.2
set label 2 'L_2' at 1.96,-0.2
set label 3 'L_3' at -2.65,-0.2
set label 4 'L_4' at 0.9,-1.55
set label 5 'L_5' at 0.9,1.25
set isosamples 200
set cntrparam levels discrete -20,-12,-8,-7,-6,-5.937,-5.5,-5.288,-5,-4.4,-4.1664,-4.0,-3.87
set xrange [-3:3]
set yrange [-3:3] 
GM0=1.0
x1=-1.0
y1=0.0
x2=1.0
y2=0.0
M1=5.0
M2=1.0
a=((x1-x2)**2+(y1-y2)**2)**(0.5)
set output 'Roche_potential.png'
splot GM0*(-M1/((x-x1)**2+(y-y1)**2)**(0.5)-M2/((x-x2)**2+(y-y2)**2)**(0.5)-(M1+M2)*(x**2+y**2)/(2.0*a**3))
```
