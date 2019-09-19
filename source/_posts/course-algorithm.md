---
title: course-algorithm lecture 1
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:37:38
tags:
- algorithm
- nju-course
categories:
- [algorithm, nju-course]
---
## 总览

![](C:\Users\Dell\Pictures\QQ截图20190522162200.png)

算法课上的问题有两种：

1. Order 涉及到序列的问题
2. Graph 可以建模为图的问题

常见的策略主要有两种：

1. Traversal: 即可以理解为穷举，对每种情况都进行考虑;

   > a. Order: Brute Force, 暴力遍历，如排序，选择，查找等问题
   >
   > b. Graph: DFS和BFS 

2. Optimization: 使用某些优化策略

   > a. Order:  Divide & Conquer
   >
   > b. Graph: Greedy & Dynamic Programming

## 对Computer和Computing的理解

Computer的强大之处在于能够高效地计算简单、程序式的工作

Computing 就三步：

1. encoding to 0,1
2. operations
3. decoding the '1's and '0's

Turning machine

## Algorithm的理解

Algorithm is the spirit of computing. 

注意是用来解决某个特定的问题

Essential issues:

- Model of computation:

  > 1. 默认采用的就是RAM Model.
  >
  > 2. 注意<font color='red'>simple opreation，这里认为基本的存储、加减、乘除都是单位1， 而实际上加减和乘法的实现的时间并不一样</font>

- Algorithm design

  > 1. 分析问题、建模问题到具体的框架、解决问题、优化问题
  >
  > 2. 正确性证明
  >
  > 3. 例子：最小公约数证明——数学归纳法
  >
  >    > <font color='blue'>输入、输出和结果类比证明</font>

- Algorithm analysis

  > 1. Criteria, 关键操作，注意看定义的是什么
  >
  >    ![1558514755914](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558514755914.png)
  >
  > 2. 时间复杂度
  >
  >    最好、平均、最坏
  >
  > 3. 空间复杂度

## 图涉及问题以及算法总结

![](1.png)

## 一些好的例子

1. Job scheduling
2. Matrix chain multiplication