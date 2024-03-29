---
title: 关于编程范式的思考
date:  2019-11-06 09:08:43
index_img: /static/8.jpg
tags:
- 物联网技术

categories:
- 杂谈

author: wangwenhai
---
本文作者：[wangwenhai] # 概要：关于编程范式的思考
<!-- more -->


# 关于编程范式的思考

---
# 面向对象
面向对象编程——Object Oriented Programming，简称OOP，是一种程序设计思想。OOP把对象作为程序的基本单元，一个对象包含了数据和操作数据的函数。

面向过程的程序设计把计算机程序视为一系列的命令集合，即一组函数的顺序执行。为了简化程序设计，面向过程把函数继续切分为子函数，即把大块函数通过切割成小块函数来降低系统的复杂度。

而面向对象的程序设计把计算机程序视为一组对象的集合，而每个对象都可以接收其他对象发过来的消息，并处理这些消息，计算机程序的执行就是一系列消息在各个对象之间传递。

---
# 典型案例
```java
class Object{
    doSomeThing()
    ....
}
//
new Object().doSomeThing()
```
## 本质上是：*对现实世界中的静态建模*

---
# 面向过程
面向过程是一种以事件为中心的编程思想，编程的时候把解决问题的步骤分析出来，然后用函数把这些步骤实现，在一步一步的具体步骤中再按顺序调用函数。

---
# 典型案例
```C
socket(...);
bind(...);
listen(...);
accept(...);
send(...);
```
## 本质上是：*在尽可能的模拟计算机运行*

---
# 面向函数
简单说，"函数式编程"是一种"编程范式"（programming paradigm），也就是如何编写程序的方法论。属于"结构化编程"的一种，主要思想是把运算过程尽量写成一系列嵌套的函数调用。

---
# 典型案例
1. 函数是一等公民
```erlang
// define
f1(F) -> F().
// call
f1(fun() -> ok end).
```

---
2. 函数调用柯里化
```erlang
// define
add(M, N) -> M + N.
// call
add(1,2).
```

```erlang
// define
add(N) -> fun(M) -> N + M end.
// call
add(1)(2).
```

---
3. 并发模型

```erlang
// run
run() ->
    receive
      Msg -> handle(Msg),
      run()
    end.
// Produce Process
spwan(fun() -> run() end).
```
## 本质上是：*对现实世界的动态建模*