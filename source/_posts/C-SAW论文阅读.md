---
title: C-SAW论文阅读
copyright: true
top: 0
reward: false
mathjax: true
date: 2020-12-17 19:18:24
tags:
categories:
---

# C-SAW: A Framework for Graph Sampling and Random Walk on GPUs

## 摘要

表示学习是机器学习的一个基本任务，自动地从图数据中学习到特征，而不是通过手动选择。代表算法有DeepWalk, node2vec和GrpahSAGE. 

这些算法首先从输入图中采样出子图。采样是一个很困难的任务，它需要并行地独立地采样出每个子图。

现有的图预处理，数据挖掘和表示学习系统都没有有效地实现并行化采样。减低了表示学习的end-to-end的表现

本文中，我们提出了NextDoor，首个设计到GPUs上的图采样方法。同时，介绍了一种高阶API

## Introduction

表示学习的目标是学习数据的特征

当运行random walks, NEXTDOOR相比于KnightKing提高了696倍；相比于GraphSAGE's sampler增加了1300倍

主要贡献：
1. A new transit-parallel机制
2. NEXTDOOR's API
3. NEXTDOOR's system, 利用transit-parallellism, 负载均衡和缓存
4. Performance evaluation


## 背景和动机

