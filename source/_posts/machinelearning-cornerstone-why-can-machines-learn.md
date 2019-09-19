---
title: machinelearning cornerstone why can machines learn
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:32:40
tags:
- machinelearning
- cornerstone
categories:
- [machinelearning, cornerstone]
---

# 5. Training vs Testing

## 5.1 DataSize
![](00020.png)
![](00021.png)
![](00022.png)

> 理解在精度下的容忍度，从而指导需要从实际情况中选择多少个样本？

## 5.2 Effective Number of Lines
> union bound over-estimating ?

如何衡量？ VC维来进行衡量，实际是哪个关注的是对数据的划分的数目。
> 只需要可以将样本可分即可。

所以不同的算法就有假设空间

> 再次理解线性函数
> 突然想到感知机的函数，感知机这里其实就是每个样本都有一个权值。
？ 在图像上，可以理解权就表示的是直线的斜率。 然后，x表示向量，即原点到这里的向量，那么进行更新就好了。
？ 如何预测的


样本点是空间分布的
为什么觉得样本点是空间分布的是可行的？
> 可以样本点在各个维度上的坐标作为特征来考虑。

2个
3个 +,-,+不可分
> 可能有6种，可能有8种；最多8种。
4个，
![](00025.png)
> 任何情况下，都最多只有8种
> 由此我们可以看出，得到了VC维的定义，在d维下存在，在d+1维下任意都不满足（考虑进行取反）。因为探究的是最多的情况

## 5.3 Effective Number of Hyperthesis



大致过一下
Why can machine learn?
因为错误率可用VC维或R复杂度来衡量

How Can Machines Learn?
算法具体是怎么做的