---
layout:     post
title:      "Gnuplot scripts"
subtitle:   "Visualization"
date:       2024-07-24 15:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
tags:
    - Gnuplot
    - Visualization
---

# Precison in Fortran
A test program

```Fortran
program test_sdprec  
implicit none  
real :: a,a0  
real*8 :: b  
real*8 :: c,c0  
  
a0=2.0  
c0=2.D0  
print*, 'a0=',a0  
print*, 'a0=',c0  
a = sqrt(a0)  
print*, 'a=',a  
b = sqrt(a0)  
print*, 'b=',b  
b = sqrt(c0)  
print*, 'b=',b  
c = dsqrt(2.D0)  
print*, 'c=',c  
print*,sqrt(a0)  
print*,sqrt(c0*sqrt(2.0))  
print*,sqrt(c0*sqrt(2.D0))  
end program test_sdprec
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQyMjMzOTE2OF19
-->