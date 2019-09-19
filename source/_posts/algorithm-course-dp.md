---
title: algorithm course dp
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:56:21
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## DP

> DP是一种思想，就是通过构建图的模型来解决。但是有一个问题就是任何问题都可以表示为合法的图结构（只要是合法的图结构），即实际问题为有向图，一定存在入度为0的点和出度为0的点。那么根据依赖，就可以通过入门级动态规划得到解决的。



然后又想一个东西。问题可以分为哪几种类？

1. 离散空间上的点？还是其他？
2. 从某种意义上来说，还有其他东西吗？



> 对吧？
>
> 其实对应的就是图论上的最优路径的问题。 感觉上没错？
>
> 又有人说动态规划就是图论上最优路的定义问题，感觉说的很没错了。



如果把问题等效为图：其实在这里将问题等效为图，实际上每个节点对应的是图的状态，注意这里的状态空间而已，有的节点的状态不一定可以认为它是能行的。但这样一想又稍微有点过分，实际上它应该跟递归分治在一类。因为从表示和记录上，它是对原问题的最优解进行分解，在解的角度上进行建模而已。

而回溯法和分支限界法才真正地从解空间入手，至少思想上是，具体是否是还得再思考。



> 又一点想法：DP不过是问题可以分解为超级简单的图结构。
>
> DP理解其实可以有多个角度，只要某个角度理解明白也就明白了。
>
> 首先，经典角度看成是解决非重复子问题。
>
> 其次，本次课新的角度即看作是一个拓扑求解序列，从源头到尽头。a->b, 往往意味着的是a的解决依赖于b，所以这里就可以考虑深度优先遍历，确保是连通的。
>
> > 1. 这里一个关键的考虑点就是BE边时，往往是需要汇聚当前的最优结果。
> > 2. 对于CE边，当结果已经得到时，有一方便的数据结构可以存储，可以直接进行查看。



？ DP能成功运行需要的条件是什么。

最优子结构还是怎么的？

理解是否图遍历问题？即根本不需要进行图遍历。

图遍历的另一种感觉就是考虑上所有

```
通用框架

Fibonacci by DP

fibDP(soln, k)
	int fib, f1, f2
	if (k<2) fib = k;
	else
		if (member(soln,k-1)==false) //member即查询子问题是否求解
			f1=fibDP(soln,k-1)
		else f1=retrieve(soln,k-1)
		if (member(soln,k-2)==false)
			f2=fibDP(soln,k-2)
		else f2=retrieve(soln,k-2)
	store(soln,k,fib)
	return fib
```



> 解题三个步骤：
>
> 1. 整体思路
> 2. 算法设计
> 3. 时空分析及正确性证明
> 4. 改进建议





### 1. Matrix Multiplication

#### v1. 递归版本



```
mmTry1(dim, len, seq)
	if len<3 bestCost=0
	else
		bestCost=\infity
		for (i=1; i<=len-1; i++)
			c=cost of muitiplication at position seq[i]
			newSeq=seq with ith elemnt deleted;
			b=mmTry1(dim, len-1, newSeq)
			bestCost=min(bestCost, b+c)
	return bestCost
```

时间复杂度： T(n) = (n-1)T(n-1)+n.  $O((n-1)!)$

问题求解结构就是一棵树的结构。

不是很清晰？

#### v2. 递归与分治

看子问题的求解方式，也可以知道递归与分治版本是没有动态规划强的。

>表面上看还是子问题的规模问题

Improved Recursion

```
mmTry2(dim, low, high)
	if(higt-low==1) bestCost=0
	else
		bestCost=\infity
		for(k=low+1;k<=high-1;k++)
			a=mmTry2(dim, low, k);
			b=mmTry2(dim, k, high);
			c=cost of multiplication at position k;
			bestCost=min(bestCost, a+b+c)
	return bestCost
```

时间复杂度： T(n) $\geq$ 2T(n-1) + O(n). $\Omega(2^n)$

#### v3. 入门级DP

用二维空间存储，子问题解。

store(cost, low, high, bestCost)

```
mmTry2DP(dim, low, high, cost)
	bestCost=0
	for(k=low+1;k<= high-1;k++)
		if (member(low,k)==false)
			a=mmTry2(dim,low,k)
		else a=retrieve(cost,low,k)
		if (member(k,high)==false)
			a=mmTry2(dim,k,high)
		else b=retrieve(cost,k,high)
		c=cost of multiplication at position k;
		bestCost=min(bestCost, a+b+c)
	store(cost,low,high,bestCost)
	return bestCost
		
```



#### v4. 高级DP

> 为什么这个好？不需要编译器进行调度
>
> 不管写法是否循环，都是用子问题的解去拼大问题的解。
>
> 循环：人为规定子问题的求解顺序。
>
> 递归：编译器决定递归的子问题求解顺序。
>
> Tips: 二维循环找调度，一维循环找最优



DP求解方法：

```
Steps for applying DP:

1.Define subproblems: # of subproblems
2.Set the goal
3.Define the recurrence
	- larger subproblem <- # smaller subproblems
	- init. conditions
4.Write pseudo-code
	fill table in some order
5.Analyze the time complexity
6.Extract the optimal solution(optionally)
```

Common subproblems in DP

- 1D subproblems

  ```
  Input: x_1, x_2, ...,x_n (array, sequence, string)
  Subproblems: x_1, x_2, ..., x_i (prefix/suffix)
  #: O(n)
  Examples: Maximum-sum subarray, Longest increasing subsequence, Text justification
  ```

- 2D subproblems

  ```
  --Class 1
  Input: x_1, x_2, ...,x_m; y_1, y2, ...,y_n
  Subproblems: x_1,x_2,...,x_i; y_1,y_2,...,y_i
  #: O(mn)
  Examples: Edit distance, Longest common subsequence
  
  --Class 2*
  Input:x_1, x_2, ...,x_n
  Subproblems:x_i,...,x_j
  #: O(n^2)
  Examples: Matrix chain multiplication, Optimal BST
  ```

- 3D subproblems

  ```
  Floyd-Warshall algorithm
  	d(i,j,k)=min(d(i,j,k-1), d(i,k,k-1)+d(k,j,k-1))
  ```

- DP on graphs

  - On rooted tree

    Subproblems:  rooted subtrees

  - On DAG

    Subproblems: nodes after/before in the topo. order

- Knaspack problem

  Subset sum problem, Change-making problem