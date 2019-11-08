---
title: gnn-parallel-ggsnn-code-realize
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-11-01 09:29:13
tags:
- gnn
categories:
- gnn
---

## 待做

1. 表示复杂的梯度计算图
    > 根据计算图来表示，计算图有两种类型
    - 节点表示操作，边表示数据，如Tensorflow
    - 节点表示数据，边表示操作，如计算图 https://blog.csdn.net/zxl55/article/details/83537144

2. 将所有复杂的公式转变为矩阵操作，然后直接使用Tensorflow类似的想法，不过感觉上可以做得更具体点

3. 剥离输入输出，模型照抄
> 为什么远程登录是用到Anaconda中的python



##### 一些常量说明
- H(t-1): 上一轮的状态
- H(t): 最终状态
- Hz(t): 当前计算的状态
- Wa: 初始化对H(t-1)做的操作
- Wz, Wh, Wr: Z(t), Hz(t), R(t)关于Av(t-1)的权重
- Uz, Uh, Ur: Z(t), Hz(t), R(t)关于H(t-1)的权重
- $Av=[A_{in}, A_{out}]$

##### 图上的操作
$Av(t-1) = Av(H(t-1)Wa)$
##### GRU单元
$Z(t) = sigmoid(Av(t-1)Wz + H(t-1)Uz)$
$R(t) = sigmoid(Av(t-1)Wr + H(t-1)Ur)$
$Hzt(t) = tanh(Av(t-1)Wh + (R(t) \odot H(t-1))Uh)$
$H(t-1) = (1-Z(t))\odot H(t-1) + Z(t) \odot Hzt(t)$

## 问题总结

1. RuntimeWarning: overflow encountered in exp
    > https://www.cnblogs.com/zhhy236400/p/9873322.html
    