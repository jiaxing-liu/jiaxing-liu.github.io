---
layout:     post
title:      "Fortran function in a module"
subtitle:   "conflicts with symbol from module"
date:       2022-08-21 19:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - Fortran
    - Module
    - Function
---

# Install PSC(Plasma Simulation Code) code on CentOS

## function out of a module 

the function need to be declared when it is used in a subroutine or main program.
```Fortran
      program test
      implicit none
      real:: f
      external:: f
      real:: x
      x=1.0
      call tests(x)
      print*,f(x)
      end program test

      subroutine tests(x)
      implicit none
      real x,f
      print*,f(x)
      end

      function f(x)
      implicit none
      real:: f
      real::x
      f=x
      end function f
```

If the function is not declared, there will be such an error
```bash
undefined reference to `__func_MOD_f```

## function in a module 

the function can not be declared when it is used in a subroutine or main program.

```Fortran
      module func
      contains
      function f(x)
      implicit none
      real:: f
      real::x
      f=x
      end function f
      end

      program test
      use func
      implicit none
!      real:: f
!      external:: f
      real:: x
      x=1.0
      call tests(x)
      print*,f(x)
      end program test

      subroutine tests(x)
      use func
      implicit none
      real x     !,f
      print*,f(x)
      end
```
If f is declared in the subroutine or main program, there will the such an error
```bash
   12 |       use func
      |         2     
   13 |       implicit none
   14 |       real:: f
      |              1
Error: Symbol ‘f’ at (1) conflicts with symbol from module ‘func’, use-associated at (2)
```
