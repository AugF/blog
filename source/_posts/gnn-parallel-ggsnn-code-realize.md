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
$Av(t-1) = A_{v:} (H(t-1)Wa)$
##### GRU单元
$Z(t) = \delta (Av(t-1)Wz + H(t-1)Uz)$
$R(t) = sigmoid(Av(t-1)Wr + H(t-1)Ur)$
$Hzt(t) = tanh(Av(t-1)Wh + (R(t) \odot H(t-1))Uh)$
$H(t-1) = (1-Z(t))\odot H(t-1) + Z(t) \odot Hzt(t)$


##### 原论文公式
$\mathbf{a}_v^{t} = \mathbf{A}_{v:}^T [\mathbf{h}_1^{(t-1)T} ... \mathbf{h}_{|V|}^{(t-1)T}]^{T} + \mathbf{b} $
$\mathbf{z}_v^t = \mathbf{\delta} ( \mathbf{W}^z \mathbf{a}_v^{t} + \mathbf{U}^z \mathbf{h}_v^{t-1})$
$\mathbf{r}_v^t = \delta ( \mathbf{W}^r \mathbf{a}_v^{t} + \mathbf{U}^r \mathbf{h}_v^{t-1})$
$\mathbf{h}_v^t = tanh ( \mathbf{W} \mathbf{a}_v^{t} + \mathbf{U} ( \mathbf{r}_v^t \odot \mathbf{h}_v^{t-1}))$
$\mathbf{h}_v^t = (1 - \mathbf{z}_v^t) \odot \mathbf{h}_v^{t-1} +  \mathbf{z}_v^t \odot \mathbf{h}_v^t$

##### 代码 pyg

$\mathbf{h}_j^{0} = \mathbf{x}_i \mathbf{\parallel} 0$
$\mathbf{m}_i^{l+1} = \sum_{j \in \mathcal{N}(i)} \mathbf{\Theta} \mathbf{h}_j^{l}$
$\mathbf{z}_i^{l+1} = \mathbf{\delta} ( \mathbf{W}^z \mathbf{m}_i^{l+1} + \mathbf{U}^z \mathbf{h}_i^{l})$
$\mathbf{r}_i^{l+1} = \delta ( \mathbf{W}^r \mathbf{m}_i^{l+1}+ \mathbf{U}^r \mathbf{h}_i^{l})$
$\mathbf{h}_i^{l+1} = tanh ( \mathbf{W} \mathbf{m}_i^{l+1} + \mathbf{U} ( \mathbf{r}_i^{l+1} \odot \mathbf{h}_i^{l})))$
$\mathbf{h}_i^{l+1} = (1 - \mathbf{z}_i^{l+1}) \odot \mathbf{h}_i^l +  \mathbf{z}_i^{l+1} \odot \mathbf{h}_i^{l+1}$



$\mathbf{M}^{l+1} = \mathbf{A} \mathbf{H}^{l} \mathbf{\Theta} $
$\mathbf{Z}^{l+1} = \mathbf{\delta} (\mathbf{M}^{l+1} \mathbf{W}^z + \mathbf{H}^{l} \mathbf{U}^z)$
$\mathbf{R}^{l+1} = \delta ( \mathbf{M}^{l+1} \mathbf{W}^r + \mathbf{H}^{l} \mathbf{U}^r)$
$\mathbf{H}_t^{l+1} = tanh ( \mathbf{M}^{l+1} \mathbf{W}  + (\mathbf{R}^{l+1} \odot \mathbf{H}^{l})\mathbf{U}))$
$\mathbf{H}^{l+1} = (1 - \mathbf{Z}^{l+1}) \odot \mathbf{H}^l +  \mathbf{Z}^{l+1} \odot \mathbf{H}_t^{l+1}$

以算$\mathbf{W}^r$的权重矩阵梯度为例，我们需要将上面公式拆分为以下小公式，
$\mathbf{T}_1 = \mathbf{M}^{l+1} \mathbf{W}^r $
$\mathbf{T}_2 = \mathbf{T}_1 + \mathbf{H}^{l} \mathbf{U}^r$
$\mathbf{R}^{l+1} = \mathbf{\delta} (\mathbf{T}_2)$
$\mathbf{T}_3 = \mathbf{R}^{l+1} \odot \mathbf{H}^l $
$\mathbf{T}_4 = \mathbf{T}_3 \mathbf{U}$
$\mathbf{T}_5 = \mathbf{M}^{l+1} \mathbf{W} + \mathbf{T}_4$
$\mathbf{H}_t^{l+1} = tanh(\mathbf{T}_5)$
$\mathbf{T}_6 =  \mathbf{Z}^{l+1} \odot \mathbf{H}_t^{l+1}$
$\mathbf{H}^{l+1} = (1 - \mathbf{Z}^{l+1}) \odot \mathbf{H}^l + \mathbf{T}_6$

从而，反向传播，通过链式法则，$\mathbf{W}_r$的梯度就为：
$\frac{\partial loss} {\partial \mathbf{W}_r} = \frac {\partial loss}{\partial\mathbf{H}^{l+1}} \cdot  \frac {\partial\mathbf{H}^{l+1}} {\partial\mathbf{T}^6} \cdot  \frac {\partial \mathbf{T}_6}{\partial\mathbf{H}_t^{l+1}} \cdot  \frac {\partial\mathbf{H}_t^{l+1}}{\partial\mathbf{T}_5} \cdot  \frac {\partial\mathbf{T}_5}{\partial\mathbf{T}_4} \cdot \frac {\partial\mathbf{T}_4}{\partial\mathbf{T}_3} \cdot \frac {\partial\mathbf{T}_3}{\partial\mathbf{R}^{l+1}} \cdot \frac {\partial\mathbf{R}^{l+1}}{\partial\mathbf{T}_2} \cdot \frac {\partial\mathbf{T}_2}{\partial\mathbf{T}_1} \cdot \frac {\partial\mathbf{T}_1}{\partial\mathbf{W}_r}$
## 问题总结

1. RuntimeWarning: overflow encountered in exp
    > https://www.cnblogs.com/zhhy236400/p/9873322.html
    

实现说明
由于该算法初始输入要求输入维度小于隐藏层的维度，所以在实现时，在Input Layer后进行了特征到隐向量维度的转换，在Prediction Layer前进行
