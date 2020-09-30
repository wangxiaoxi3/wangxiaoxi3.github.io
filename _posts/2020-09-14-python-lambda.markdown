---
layout: post
title: Python-匿名函数
date: 2020-09-14 14:30:00 +0900
categories: Python
---

匿名函数的关键字是 lambda，之后是一系列的参数，然后用冒号隔开，最后则是由这些参数组成的表达式。
```
lambda argument1, argument2,... argumentN : expression
```

#### 第一:lambda 是一个表达式（expression），并不是一个语句（statement）。

```
square = lambda x: x**2
square(3)
运行结果：
9
```
#### 第二:lambda 的主体是只有一行的简单表达式，并不能扩展成一个多行的代码块。
#### 第三:为什么要使用匿名函数？
* 减少代码的重复性
* 模块化代码

