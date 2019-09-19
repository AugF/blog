---
title: algorithm course BruteForce
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:04:00
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---

## 1.1 数学归纳法溯源与公理化思维

> 首先我们需要考虑到自然数的定义到底是什么。
>
> 一般地，定义$\empty $为0，注意这里参考了维基百科
>
> 

1. well-ordering principle

   > 任何非空的自然数集合必然有最小元素，既是最基本的原则。

2. 只有一种数学归纳法

   > 基于良序定理，考虑一组关于自然数的命题P(n). 如果P(n)不是关于所有的自然数都成立，那么必然有P(n)存在最小反例。
   >
   > 如果P(n)不对所有命题都成立，则
   >
   > $$\exist a \geq 2, P(1) \and P(2) \and ... \and \neg P(a) = True$$
   >
   > 否命题， P(n)对所有命题都成立
   >
   > $$\exist a \geq 2, P(1) \and P(2) \and ... \and \neg P(a) = FALSE$$
   >
   > 逆否命题
   >
   > $$\exist a \geq 2, P(1) \and P(2) \and ... \and P(a-1) \rightarrow P(a) =True$$
   >
   > 

3. 一个错误的数学归纳法证明
4. 算法正确性证明实例

## 1.2 极限、实数与$\epsilon -N $

$\mathbb{N} \Rightarrow \mathbb{Z}$:   减法不封闭

$\mathbb{Z} \Rightarrow \mathbb{Q}$:   除法不封闭

$\mathbb{Q} \Rightarrow R$:   无限，无穷概念的引入

$\mathbb{R} \Rightarrow \mathbb{C}$:  引入复数域，由于是否存在根的原因 

## 1.3 从算法角度重新审视数学的概念

#### 1. 一些基本概念

- 自变量， 通常是自然数

- 单调性？

- 取整 $\lceil x \rceil , \lfloor x \rfloor$, 问题的量往往是自然数级别的

- 对数$\log{n}$

  > 与之相关的现象：
  >
  > 1. 折半，当问题划分规模为n的问题，经过折半查找，大概$\log{n}$次循环问题的规模降为常数。
  >
  > 2. 完美二叉树，对于一个n个节点的完美二叉树，他的高度为$\lfloor \log{n} \rfloor$.
  >
  >    > - 完美二叉树的概念
  >    >
  >    >   所有外部节点的深度都相同
  >    
  > 3.  二进制数的比特数。一个十进制的自然数的二进制表示所需的比特数为$\lfloor \log{n} \rfloor + 1$.
  >
  >    > 从数值的角度，二进制数每向右移动一位，比特数减1，数值变为原来的$\frac{1}{2}$
  
- 阶乘

  > 对于n个完全不同的数，其全排列的个数为$n!$.
  >
  > 如果阶乘是连乘形式，通常对它取对数变成求和形式。
  >
  > 这里有个Stirling公式，因为平时用的不多，这里就不补充了

  

### 2. 常用级数求和

求和与算法分析往往关系非常密切。

- 多项式级数
  1. $\sum_{i=1}^n {i} = \frac {n(n+1)}{2}$
  2. $\sum_{i=1}^n i^2 = \frac{n(n+1)(2n+1)}{6}$
  3. $\sum_{i=1}^n i^k = \Theta (\frac{1}{k+1} n^{k+1})$

- 几何级数，等比数列，只考虑最大的那项即可

- 算术几何级数

  > $\sum_{i=1}^k i *2^i = (k-1) 2^{k+1} + 2$

- 调和级数

  > $\sum_{i=1}^k \frac{1}{i} = \ln k + \Upsilon + \epsilon$

- 斐波拉数列



### 3. 期望值， 指标随机变量，期望的线性特征

### 4.  <font color='red'>蛮力算法</font>



1. 微博名人问题: 寻找图中所有人都关注，但是不关注别人的人。

   > BF1: 对每个人是否是名人进行判断，时间复杂度为$O(n^2)$
   >
   > BF2: 对每一对关系进行思考，根据一对关系，进行将名人进行筛选
   >
   > 本质：集合中的二元关系，所以有算法下界O(n^2) ??

2. 频繁项问题特例(出现次数超过一半)的线性时间解

   > 注意这里实际利用了$f \geq n/2$ 这件事，扩展思考 **$f \geq n/k$**

3. 候选：交换左右部分，最大和连续子串问题，Maxima问题的非分治解
