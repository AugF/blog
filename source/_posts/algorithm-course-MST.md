---
title: algorithm course MST
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:02:26
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---

## MST

> 注意，MST从某种意义上是图上的应用关于贪心策略的一部分。

### Definition

定义1. 常规定义： X, S, V/S, e: a lightest edge across (S,V/S)



证明要点：

首先假设cut中的一条e在MST中，然后因为cut中有一条cycle，所以必然有一条边可以构成。所以可以构成一条最小生成树，即取cycle中最小的边。

MST每次都有一部分。



定义2.  Reverse-delete方法

delete an 最大权edge if this does not disconnect the graph.

Cycle Property

$T \in F \Leftrightarrow \exist T': T' \in F - {e}  $



定义3.  



10.18 求证：Minimum-weight eage across any cut is unique , so Unique MST.

证明1：

Construct T by adding all such edges.

$e \in \forall MST, T \in \forall MST$

T is spanning tree eage.

- construct cycle 连通
- no cycle ?

证明2：

依赖于某个算法进行证明。



### Code

更多地思考

使用递归与分治的策略解决MST问题

>  整体思路:
>
> 1. 将图按点集划分为两个集合，分别求解这两个集合的MST。
> 2. 然后再找这两个集合之间的最小边，将两个MST一起合并为整个图的MST。
