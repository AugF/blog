---
title: algorithm course bfs
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:52:40
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## BFS

> 注意BFS这里使用的队列所具备的性质：
>
> 设队列首部出元素，尾部入元素。从首到尾的元素为：v1, v2, ..., vr
>
> 则有： v_i.dis <= v_{i+1}.dis,  v1.dis <= vr.dis + 1



另外注意这里每个点染色的具体含义：

- white:  未进入队列
- gray: 在队列中的元素
- black: 出队列的元素



1. 统计变量

```
struct Vertex{
	string color;
	int parent;
	int dis; // dis这里实际就可以理解为所在的层数
}

BFS-wrapper(){
	Vertex[] V = new int[n+1];
	Edge[] E = new int[m+1];
	
	for v in V:
		v.color = white;
	
	for v in V:
		if v.color = white:
			v.parent = -1;
			BFS(v);
}
```



2. 有向图

> 注意到这里BFS, 实际上这里是非递归写法，所以改变颜色的状态注意要在循环内部改变。



> 注意，对于BFS，不可能会有DE的存在，反证法，假设各种颜色。还有就是对于有向图，uv为CE，满足u.dis 

```
BFS-directed(v){
	v.color = gray;
	v.dis = 0;
	queue.enqueue(v);
	while (!queue.isEmpty){
		w = queue.dequeue();
		for x that wx in E:
			if x.color = white:
				x.color = gray;
				x.parent = w;
				x.dis = w.dis + 1:
				queue.enqueue(x);
			if x.color = gray:
				// CE: 恰好在不同的两层
			if x.color = black:
				// if root(w) == x: BE
				// else if w,x在同一层 CE
		
		w.color = black;
	}
	
	v.color = black;
}
```



### 无向图



> 1. 无向图的BE不存在（还未进入队列时，应该已经被当做TE处理了），DE同样也不存在，可以理解为BE和DE同一类
> 2. CE不可能为黑色，此时一定是二次遍历，之前已经遍历过

```
BFS-undirected(v){
	v.color = gray;
	v.dis = 0;
	queue.enqueue(v);
	while (!queue.isEmpty){
		w = queue.dequeue();
		for x that wx in E:
			if x.color = white:
				x.color = gray;
				x.parent = w;
				x.dis = w.dis + 1:
				queue.enqueue(x);
			if x.color = gray: // CE
				x.dis = w.dis or w.dis + 1;
		w.color = black;
	}
	
	v.color = black;
}
```



另外一个思考，BE边有用到，DE，CE边在哪些问题中会用到和处理呢。



## 应用

1. 有向图



2. 无向图

- 判断二分图
- 寻找k度子图