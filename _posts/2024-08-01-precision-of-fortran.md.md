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
c = dsqrt(c0)  
print*, 'c=',c  
print*,sqrt(a0)  
print*,sqrt(c0*sqrt(2.0))  
print*,sqrt(c0*sqrt(2.D0))  
end program test_sdprec
```
`gfortran test.f90` 的输出结果为
```Fortran
a0= 2.00000000  
a0= 2.0000000000000000  
a= 1.41421354  
b= 1.4142135381698608  
b= 1.4142135623730951  
c= 1.4142135623730951  
1.41421354  
1.6817928161160998  
1.6817928305074292
```
`gfortran -fdefault-real-8 test.f90` 编译时有如下报错
```Bash
9 | c = dsqrt(c0)  
| 1  
错误: ‘x’ argument of ‘dsqrt’ intrinsic at (1) must be double precision
```
`c = dsqrt(c0)` 改为 `c = dsqrt(2.D0)` 重新编译通过，输出结果为
```Bash
a0= 2.0000000000000000  
a0= 2.0000000000000000  
a= 1.4142135623730951  
b= 1.4142135623730951  
b= 1.4142135623730951  
c= 1.4142135623730951  
1.4142135623730951  
1.6817928305074292  
1.68179283050742908606225095246642969
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMDg1MDQ3OTddfQ==
-->