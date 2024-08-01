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
## test 1
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
```Bash
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
说明

- 加上 `-fdefault-real-8` flag 后，2.0 也被认为是双精度。（根据倒数第一次第二行、第二次倒数第一行的输出）
- 为和报错不清楚？？？

## test 2
```Fortran
program test_sdprec  
implicit none  
real*8 :: c,c0  
real*16 :: d  
c0=2.D0  
c = dsqrt(2.D0)  
!c = dsqrt(c0)  
print*, 'c=',c  
print*, 'c0=',c0  
print*,sqrt(c0*sqrt(2.0))  
print*,sqrt(c0*sqrt(2.D0))  
print*,sqrt(c0*sqrt(2.D0))  
d=c0*sqrt(2.D0)  
print*,sqrt(d)  
end program test_sdprec
```
`gfortran test.f90` 编译运行输出结果
```Bash
c= 1.4142135623730951  
c0= 2.0000000000000000  
1.6817928161160998  
1.6817928305074292  
1.6817928305074292  
1.68179283050742914354432090834860257
```
前两行没什么问题；第3、4行不一样，是因为 2.0 默认为单精度，2.D0 为双精度；
`gfortran -fdefault-real-8` 编译运行输出结果
```Bash
c= 1.4142135623730951  
c0= 2.0000000000000000  
1.6817928305074292  
1.68179283050742908606225095246642969  
1.68179283050742908606225095246642969  
1.68179283050742908606225095246642969
```
前两行没问题；第3行和上面第四行一样，说明 -r8

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIzNDgxNTMyNiw1MjI4MDA1MDhdfQ==
-->