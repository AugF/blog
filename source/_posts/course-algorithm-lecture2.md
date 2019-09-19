---
title: course-algorithm lecture2
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:43:05
tags:
- algorithm
- nju-course
categories:
- [algorithm, nju-course]
---

## L2. Asympototic

###  1. growth rate of functions

1. 用关键操作的数量来进行衡量

![1558515224097](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558515224097.png)

> 1. $f \in O(g) \Leftrightarrow f(n) \leq kg(n)$
>
>    > 这里 $lim_{n \rightarrow \infty} \frac{f(n)}{g(n)} < \infty$  注意可以**不存在**或者等于0

<font color='red'>注意对于O来说，极限可以不存在；对于\omega来说可以为无穷大</font>

2. 增长率级别的一个概念

> 由小到大：$logn, n, nlogn, n^k, 2^n, n!$
>

3. 一些经验数据，数量级与时间和空间的感觉

### 2. Brute Force的思考

例子1： Swapping array elements: 交换数组中的所有元素

<time, space>

1. BF1 <O(n^2), O(1)>

   >  冒泡的思想，即把第一个元素不断冒泡冒泡到最后一个元素去。

2. BF2 <O(n), O(n)>

   > 对辅助空间进行处理

3. ？ <O(n), O(1)>

   > 不断对元素进行交换的思想

例子2： Maximum subsequence sum: 寻找最大子序列和

1. BF1 $O(n^3)$

   > 最朴素的思想： 定最左边的限度，在动态寻找右边的限度，再计算片段和
   >
   > ![1558538541074](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558538541074.png)

2. BF2 $O(n^2)$

   > 定左边的限度后，对于右边实际上不用定完限寻找片段和。
   >
   > ![1558538558913](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558538558913.png)

3. 分治 $O(n\log{n})$

   ![1558538619802](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558538619802.png)

4. 线性

   ![1558538654841](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558538654841.png)

## L3. Recursion

### 1. Recursion

特别想说明的就是，通过学习了计算模型导引后的感觉就是对于任何的函数都可以写成递归式，如以下几种方式：

1. 本原函数：零函数、后继函数、投影函数
2. 复合算子，减法，u算子，max算子

最终写成带参递归方式，即

```
h(x,0) = f(x)
h(x,y+1)=g(x,y,h(x,y))
// 其中x可以为向量
```

但是在算法中的常见形式为：

```
M(1) = 1
M(n) = 2M(n-1)+1
```

然后对于该算法分析就可以用解生成函数的方法来解。

### 2 . Divide and Conquer

- Divide: Divide the "big" problem to smaller ones

- Conquer: Solve the "small" problems by recursion

- Combine: Combine results of small problems, and solve the original problem

  > <font color='red'>对于我来说，这块常常是我的难点。问题在于务必请想清楚这里原问题的解和这里合并的解是否相同</font>

```
divectlySolve(I) {}

combine(S1，..., Sk) {} // 时间复杂度为f(n)

// k--- the number of divide parts, 设为c
// the number of subquestions: size(I)/size(Ii)，设为b

solve(I) {
	n = size(I)
	if (n <= smallSize)
		solution = directlySolve(I);
	else
		divide I into I1, ..., Ik
		for each i \in {1, ..., k}
			Si = solve(Ii);
		solution = combine(S1, ..., Sk)
	return solution
}
```

算法分析的常见形式：

```
T(n) = bT(n/c) + f(n)
T(1)可以在常数时间解决
```

### 3. 对比

- Recursion,

  > 粒度： n, n-1, n-2,...

- Divide and Coqueer

  > 粒度： n, n/2, n/4, ...

递归形式都可以统一为

```
T(n) = bT(n/c) + f(n)
T(1)可以在常数时间解决0
```



求解时间复杂度：

1. 解递归方程

```
最小阶看作0，其余看作x的多少次方，然后待定系数法。再根据常数时间做出来。
```

2. 先猜，然后数学归纳法进行证明。

特别地，<font color='red'>Master定理</font>, 令$E = log_c b$

![1558532639381](C:\Users\Dell\AppData\Roaming\Typora\typora-user-images\1558532639381.png)

### 4. Examples

- Maxima 找最大的数
- Frequent element 找出现次数最多的数
- Multiplication 大整数相乘
- Nearest point pair 最邻接点



Recursion vs Induction 归纳与演绎

Smooth functions: 非负非减函数，对于此类函数满足$f(bn) \in O(f(n))$

