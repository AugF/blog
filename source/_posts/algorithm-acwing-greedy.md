---
title: algorithm acwing greedy
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:13:13
tags:
- algorithm
- acwing
- greedy
categories:
- [algorithm, acwing, basic, greedy]
---
# 贪心
无套路而言，最难的是证明其正确性

试下还是有套路的
一般来说，举例看是否能证明正确性

一般区间问题，先排序
1. 按区间左右端点排序，或者双关键字排序

## 区间选点
> 数轴选最少的点覆盖所有的区间

1. 将每个区间按右端点从小到大排序
2. 从前往后依次枚举每一个区间
如果当前区间已经包含点，则直接pass
否则，选择当前区间的右端点

A=B A<=B A>=B

Ans问题的最优解  ans实际问题的解
最小值

Ans <= cnt
> cnt是一种可行方案，而Ans是所有可行方案中最小的。 显然!
Ans >= cnt
> 找到了cnt相互没有交集的区间，每选一个点最多覆盖一个区间。所有可行解必定大于cnt
> 关键证明，算法得到的是最基本的情况！！！

## 最大不相交区间数量

贪心策略： 按右端点选取
最大值

Ans >= cnt: 证明cnt是可行方案， 
Ans <= cnt:
假设Ans>cnt, 反证法

Ans选择出的是两两没有交集的区间，那一定被第一题的解给覆盖，所以Ans应该<=cnt的. 所以矛盾

> 考虑问题的转化，记住简单问题证明的模板，把其他复杂的问题转化成原本的问题

## 区间选组

贪心：左端点
1. 将所有区间按左端点从小到大排序
2. 从前往后处理每个区间
    判断能否将其放到某个现有的组中 如果L[i]<= min{r},则不能加入，于是建立新组；否则加入并更新

考虑数据结构: 

问题： 最小组数
证明：
1. Ans <= cnt
cnt是可行解, 每个组内都没有交集
2. Ans >= cnt
当与任何一个区间都没有交集

cnt-1  Max_r >= Li.  因为是按L排序，所以Li>=L0~i-1

意味着整个cnt组都有公共点，所以对这些基本的组来说，问题的解至少得花cnt, 因此Ans>=cnt

> 练习题
https://www.acwing.com/problem/content/113/

> 左端点相交的最大个数，右端点不相交的最大个数
> 再练习一遍


## 区间覆盖

用最少的区间将给定区间排序

贪心：将左端点从小到大排序

1. 将所有区间从左端点从小到大排序
2. 从前往后依次枚举每个区间，在所有能覆盖start的区间中，选择一个右端点最大的区间; 此时更新新的start点

Ans<=cnt: 假设有解, 因为我们每次选择区间都是没有空隙的，所以一定能找到方案成为可行解。因此成立

Ans>=cnt:

反证法，假设Ans < cnt

假设不一样，那么第一个不同的区间，Ans取得的右端点的值小于cnt取得的区间的右端点的值; 那么Ans选择的下一个区间一定与当前区间覆盖。即最优解中选出的区间一定能够替换成算法的区间。而且总的Ans数不会变，而且可能Ans大于cnt。


任何一个最优解都可以通过等价变化得到算法的解，而且每次的变换都不会增加区间的数量，所以算法最终得到的cnt数量一定和Ans相等

## 合并果子

叶节点被算的次数 = 层数
如何合理的安排使得最终的权值最小？
每次贪心地选择两个最小的数进行合并

1. 最小的两个点一定深度最深，且可以互为兄弟。
> 如果有多个兄弟可以互相交换，并不影响结果；意味着第一步选择两个最小的元素是对的！

如何证明？
交换任意两个节点，一定会使结果变大。所以当前策略采取的一定是最优的

2. n个点是最优解，如何证明n-1的最优解是n的最优解
f(n)=f(n-1)+a+b
> 因为第一次一定选a,b两个点，所以代价必然是这个
目标是求f(n)的最小值
> 因为所有的方案都是合并a和b,所以这里f(n)就等价于f(n-1)
因此，数学归纳法就成立


耍杂技的牛

一种想法就是如何进行排列？
1. 猜想1： 根据左端点
2. 猜想2： 根据右端点
3. 猜想3： 根据左右端点的加和
Min
贪心得到的答案 > 最优解

贪心得到的答案 <= 最优解
> 假设不是，调整得到最优
> 证明的过程就是比较值，然后比较的技巧就在于抛去一些无关因素的影响