---
title: neu algorithm course
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:23:01
tags:
- algorithm
- neu
- course
categories:
- [course, neu, algorithm]
---

## NEU-Algorithm

主要是为了对比这学期学的算法书与本科学习的清华大学之间教材的区别。

清华大学教材

1. 递归分治
2. 动态规划
3. 贪心策略
4. 回溯法，一种复杂版的蛮力的感觉
5. 分支限界法
6. 随机化算法
7. 线性规划与网络流



还有一个思考点：

常规思路：

问题， BF1， BF2， 改进，......



大量的题之后

1. 对于某道题，什么样的解法能将其搞定？什么样的解法能直觉达到最优
2. 这些方法的适用范围以及时间复杂度



### 4. 回溯法

> 从某种意义上感觉，回溯法是用来明确定义了问题的解空间。比如背包问题，有n个物品，则解空间就是{0,1}^N。 但这样下来感觉上有一个问题就是时间复杂度与n有关，不也不一定这样说，应该是说解空间为最多可以容纳多少个解。
>
> ！！！这里就涉及到问题的表示方法，即一个问题究竟怎样可以表示地最优

利用了DFS框架，首先将代解的问题看作一棵搜索树。

#### 1）对应的问题定义是什么？

问题的解向量：回溯法希望一个问题的解可以表示为一个n元式(x1,x2,...,xn)的形式

显约束：对分量Xi的取值限定

隐约束：对于问题的一个实例，解向量满足现实约束条件的所有多元组，构成了该实例的一个解空间。

注意：同一个问题可以有多种表示n，有些表示方法更简单，所需表示的状态空间更小（存储量少，搜索方法简单）p(n)

#### 2） 相关的一些定义

扩展节点：正在产生儿子的节点，即第一类灰色节点，当前最小灰色节点

活节点：一个自身已生成但其儿子还没有全部生成的节点， 即第二类灰色节点，非直系灰色节点

死节点：黑色节点

#### 3）实现方法

问题状态生成方法：

如果对一个扩展节点R，一旦产生了它的一个儿子C，就把C当做新的扩展节点。在完成对子树C的穷尽搜索之后，将R重新变成扩展节点，继续生成下一个儿子。

回溯法：为了避免发生那些不可能产生最佳解的问题状态，要不断地利用限界函数（bounding function）来处死那些实际上不可能产生所需解的活节点，以减少问题的计算量。

具有限界函数的深度优先方法称为回溯法

#### 4）基本思想

- 针对所给问题，定义问题的解空间

- 确定易于搜素的解空间结构

- 以深度优先方式搜素解空间，并在搜素过程中用剪枝函数避免无效搜素。

  > 常用剪枝函数：用约束函数在扩展节点处剪去不满足约束的子树；用限界函数剪去得不到最优解的子树。
  >
  > 即两个条件：
  >
  > - 每个值本身的约束
  > - 是否能得到最优解的约束

#### 5）时空复杂度分析

空间：由于在搜素过程中是动态产生问题的解空间。所以，在任何时刻，算法只保存从根节点到当前扩展节点的路径即可。所以如果解空间树中从根节点到叶节点的最长路径的长度为h(n)，那么回溯法所需的计算空间通常为O(h(n))。而显式地存储整个解空间需要O(2^{h(n)})或O(h(n)!)内存空间。

时间：

> 待处理？
>
> 两种分析方法，第一种，从树出发，即本身形成的结构，然后通过树的节点来分析复杂度；第二种，由代码出发，即代码往往能表示很多东西，从代码出发可以
>
> 感觉上是 O(2^n), O(n!),  这里n应该是最小表示来说
>
> 基于全局，从全局的所有结构中寻找结果，那么可以发现其性能复杂度是高于分支限界法的。
>
> 跟动态规划一样是万能解的感觉？

#### 6） 代码框架

```

backtrack-wrapper(){
  bestw = 0
  backtrack(1);
  cout>>bestw>>endl;
}


void backtrack(int t)
{
	if(t>n || solution(x)) {   // 注意这里t>n, solution(x)应该在上一层更好，但是这里也没事。
		temp = output(x); //已经搜索到叶节点; 这里只是输出的当前可行解。
		if (temp > bestw)
			update bestw, bestx;
	}
	else
		// 排列树，例如任务的调度问题求最节约时间的任务调度，那么实际上求得的结果就是任务的排列。在这里做时考虑将边作为任务。
		for(int i=t;i<=n ;i++ )  //排列数由边构成
			// 这里说明有n个孩子，注意问题已经发生改变
			<对x[i]相关统计量进行处理> 
			// 前面的选择在x[i-1]中。
			x[i] = i; // 注意这里需要进行定义
			if (问题仍然未得到解决)
			{
			swap(x[t],x[i]);
			if (legal[t]) backtrack(t+1);
			swap(x[t],x[i]) // 进行恢复
			}
			<对x[i]相关统计量进行恢复>
// 输出 backtrack(1) n=2
// 
		
		// 子集树
		if(第一颗子树) {
				//动态变化的结果，等于说这里其实存储的是临时第t层的; 这里是遍历子树
				// 这里会产生带到下一层的变量, 比如说当前使用量cw
				if(constraint(x[t])&&bound(x[t]))  		                          backtrack(t+1);
				//这里是进行子树回溯。为下一阶段同等的子节点做准备		
		}
		if(第二颗子树) {
				...
		}
		
		for(int i=f(n,t);i<=g(n,t);i++)  // 所以这里循环的次数就是子节点的个数。
			x[t]=h(i);  h(i)为从f(n,t)到g(n,t)的选择，比如0,1
			具体来说，这里应该再增加一个进行可以对各个子节点记性处理的操作。（
			注意！该操作为什么要加在这里，肯定是在这里啊，想想递归思路）
			if(constraint(x[t])&&bound(x[t]))  		                          backtrack(t+1);
			// 这里可以加某一统计变量，比如说或选址少1啦
}

n表示最大递归深度
t表示当前递归深度
i为当前节点对应的子节点
f(n,t): 当前扩展节点未搜索过的子树起始编号；难道还要重新搞起始节点？？？
g(n,t): 当前扩展节点未搜索过的子树终止编号
h(i)为第i个可选的问题表示
x[t]这里是临时变量，可以替代很多情况的。

constraint: 关于x[t]的约束，只有当前节点的值有关。
bound: 关于能否产生解的约束，与x[t]有关，实际上还与前面的某个统计量有关。这里开始有点联想到那个什么矩阵,一般与剩余量有关。

n为全局变量，需要提前得出来。
bestw也为全局变量

思考点： 
1. 进行进一步理解？有啥意义
  进行问题结果建模，其中产生了问题结果状态树。所以此时时间复杂度分析是树节点的个数，同时需要考虑其他那些附加函数的复杂度。如果把他们看作1的话，易于分析。
2. 与DFS框架来说有什么关系？
  这里就是一种简单的树的结构，即仅仅只是对树进行DFS遍历而已。而DFS的精华在于对图的处理上。
  

进一步求解最优解和最优路径！
```

7）其他

优点本质：

回溯法的约束条件是所有解都应满足的条件，所以从某种意义上来说回溯法可以得到所有解，其中包括最优解。

> 这个跟图遍历不一样哟。

但无限界bound函数时，该问题是判定问题，不是最优化问题

8）例子

按解空间为子集树和排列树进行划分

- 子集树

  > 为什么叫子集树？答案的解是问题的解的子集。
  >
  > 问题：
  >
  > 一批n个集装箱要装上重量为c1和c2的轮船，每个集装箱i的重量为wi。且有
  >
  > $\sum_{i=1}^n w_i \leq c_1 + c_2$.
  >
  > 试图找出一种方案满足：
  >
  > 1. 首先将第一艘轮船尽可能装满
  > 2. 将剩余的集装箱装上第二艘轮船
  >
  > 目标：bestw， 
  >
  > 其余设置量：cw, bestx,  r
  >
  > 解题：
  >
  > 框架的层数对应n。 solution
  >
  > 根节点有两颗子树，选或不选（0/1）
  >
  > 可行性约束函数：$\sum_{i=1}^n w_i x_i \leq c_1$
  >
  > 上界函数（不选择当前元素）：
  >
  > 当前载重量cw+剩余集装箱的重量r $\leq$ 当前最优载重量bestw
  >
  > ```
  > *W
  > *x
  > *bestx
  > bestw
  > cw = 0
  > 
  > backtrack-wrapper(){
  > 	backtrack(1);
  > }
  > 
  > backtrack(int t){
  > 	if(t>n || solution(x)) {
  > 		查看是否需要更新
  > 		*bestx = *x;
  > 		bestw = cw;
  > 	} else {
  > 		if(cw + x[t] <= c) // 搜索左子树
  > 			x[t] = 1;
  > 			cw += x[t];
  > 			backtrack(t+1);
  > 		if(cw + x[t] > c) // 搜素右子树
  > 	}
  > }
  > ```
  >
  > 

  

- 排列树

  > 为什么叫排列树？答案的解是问题的解的排列。
  >
  > 问题:
  >
  > 批处理作业调度：给定n个作业的集合{$J_1,J_2, ..., J_n$}. 每个作业必须先由机器1处理，然后由机器2处理。作业$J_i$需要机器j的处理时间为$t_{ji}$。要求对于给定的n个作业，制定最佳作业调度方案，使其完成时间和达到最小。
  >
  > 例子：
  >
  > | $t_{j i}$ | j1   | j2   |
  > | --------- | ---- | ---- |
  > | i1        | 2    | 1    |
  > | i2        | 3    | 1    |
  > | i3        | 2    | 3    |
  >
  > 数据结构：
  >
  > 存储: $M[i][j]$, 注意i表示第i个作业，j表示机器j
  >
  > 注意这里有个问题就是j1上一个结束就可以执行下一个，而j2的话必须是上一个任务结束，下一个任务才能开始。所以数据结构
  >
  > f1: 当前任务结束时间
  >
  > f2*: f2[i] = max(f2[i-1], f1) + t2(i-1)
  >
  > ? 这题是否有问题？感觉并不需要最终相加啊
  >
  > 则由此可给出回溯法版的算法
  >
  > ```
  > inital M;
  > bestf
  > *f2
  > f1
  > f
  > *x
  > *bestx
  > 
  > backtrack-wrapper()
  > {
  > backtrack(1);
  > }
  > 
  > backtrack(int t){
  > 	if(t>n) { // 已经将事情安排完
  > 		*bestx=*x
  > 		bestf=f
  > 		//
  > 	}
  > 	else{
  > 		for(int j=t;j<=n;j++){
  > 			f1 += M[x[j]][1];
  > 			f2[i] = Max(f2[i-1], f1)+M[x[j]][2];
  > 			f += f2[i];
  > 			if(f < bestf){
  > 				swap(x[t],x[i]);
  > 				backtrack(t+1);
  > 				swap(x[t],x[i]);
  > 			}
  > 			f1 += M[x[j]][1];
  > 			f2[i] = Max(f2[i-1], f1)+M[x[j]][2];
  > 			f += f2[i];
  > 		}
  > 	}
  > }
  > ```
  >
  > 



其他例子

- 最大团问题

> 感觉上进入右子树的时机给的很有问题。

- 图的m着色问题

> 教训就是要区分判定问题还是最优化问题

#### 7）回溯法效率分析

回溯法取决于以下因素：

- 产生x[k]的时间
- 满足显约束x[k]值的个数
- 计算约束函数的时间
- 计算上界函数的时间
- 满足约束函数和上界函数约束的所有x[k]的个数

> 由此可见，在选择约束函数式通常存在生成节点数与约束函数计算量之间的折中。

### 5. 分支限界法

利用BFS框架

试图去寻找问题的一个解或者最优解。

> 在分支限界法中，每个活结点只有一次机会成为扩展节点。活节点一旦成为扩展节点，就一次性生成器所有儿子节点。在这些儿子节点中，导致不可行解或导致非最优解的儿子节点被舍弃，其余儿子节点加入活节点链表。
>
> 感觉上有点像BestFS的框架的感觉，比较奇妙的以及感觉需要深入思考的就是这里怎么把一些兄弟节点给排除的。
>
> 剪枝策略：在算法扩展节点中，一旦发现一个节点的下界不小于当前找到的最短路长，则算法剪去以该节点为根的子树。所以从某种意义上来说是等于还是在基于之前的信息进行剪枝。 所以这里的剪枝函数必然是某种与路径相关的变量。



2）算法框架

```

pq;
cw;
bestw;

branchBound(s)  // 注意s可以是随机开始的一个点
{
	pq Q;
	Q.add(-1);
	Ew=0;
	bestw=0;
	while(true) {
		for every w of v:
			if(bound(w)) Q.add(w,..);//注意这里可以添加一些其他变量之类的东西。
		// 注意这里可以做一些更新操作。
		Q.delete(v);
		if(v == -1) {
			if(Q.isEmpty) return bestw;
			Q.add(-1);
			Q.delete(Ew);
			i++;
		}
	}
}
```

3）算法直观感觉

分支限界法常用来解离散最优解的问题。

算法过程：首先，队列中有第一个元素。然后，对队列中选取的节点的子节点加入到队列中。按照优先级选取某个元素，凡是操作某个元素，对应的临时变量进行更新；当到达叶子节点时，说明开始取得某个最优值。所以此时考虑对队列中小于最优值的进行剪枝。

一个问题就是对于离散树和排列树两种不同的情况应该如何考虑。

- 单源最短路径

```
Graph {
n; //顶点数
*prev //前驱顶点数组
**c  // 邻接矩阵
*dist //最短距离数组
}

MinHeapNode{
i; // 顶点编号
length; //当前路长
}

main(){
ShortestPath(s)
// 找出s到其他所有顶点的最短路径。结果在*dist中。
}
ShortestPath(int v)
{
MinHeap<<MinHeapNode>> H(100);
MinHeapNode E;
E.i=v;
E.length=0;
dist[v]=0;
while(true){
for(int j=1; j<=n;j++){
	if(c[E.i][j]<inf && E.length+c[E.i][j]<dist[j]) {
	// 第一个条件表示有边存在， 第二个情况表示比当前的dist[j]还要短
		dist[j] = E.length+c[E.i][j];
		prev[i]=E.i;
		// 加入活节点列表
		MinHeapNode N;
		N.i =i;
		N.length=dist[j]; // 注意这里的更新
		H.Insert(N)
	}
	try {H.DeleteMin(E);} // 这里蕴含了取下一个节点。
	catch(OutOfBounds){break;}
}
}
}

```

> 分支限界法有种感觉就是利用层次遍历，首先取一个节点，然后找它的儿子节点。对每个儿子节点扩充，因为是队列，所以处理起来感觉像是并行处理。如果两个节点扩展到同一个节点，那么选取最优的节点，而删除非最优的节点。
>
> 从某种意义上说，当源点的多条路遍历结果到达某个节点时，进行比对，选择最优的。
>
> 而跟bestfs不同的是，bestfs比较是同时发生一下选定的，而分支限界法是异步发生，异步选定的。



### 6. 随机化算法

> 随机化算法的特征是对所求解问题的同一个实例用同一随机化算法求解两次可能得到完全不同的效果。但是会在很大程度上降低时间复杂度。
>
> 常见的随机化算法：
>
> 1. 数字随机化算法，常用于数值问题的求解
> 2. 蒙特卡洛算法，常用于求问题的精确解
> 3. 拉斯维加斯算法，不会得到不正确的解
> 4. 舍伍德算法，求解的算法总是正确的。