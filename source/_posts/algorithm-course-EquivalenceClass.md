---
title: algorithm course EquivalenceClass
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:58:58
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---

## Union-Find

> 主要是解决等价类关系的，一般来说是具有代表元素的。
>
> 两个操作：1. is   2. make



```
// T, V
// 采用有根数作为数据结构

IS si == sj:   
	find(si)==find(sj)
MAKE si==sj:    
	t = find(si) // si的root is t
	u = find(sj)
	union(t,u)

并查集的使用
1. 判断是否有圈
对于每条边，MAKE si==sj即可，然后再IS si==sj判断是否在圈上。

find(v){ // 寻找树的根节点
	if v.parent == -1: return v;
	else return find(s.parent);
}

union(t,u){ // 考虑特别简单的情况
	s1 = find[t];
	s2 = find[u];
	h1 = tree[s1].height;
	h2 = tree[s2].height;
	if h1 > h2:
		s2.parent = s1;
		tree[s1].height = h1 + h2;
		delete tree[s2]; //注意这里应该是s2失效
	else if h1 <= h2:
		s1.parent = s2;
		tree[s2].height = h1 + h2;
		delete tree[s1];
}
```
