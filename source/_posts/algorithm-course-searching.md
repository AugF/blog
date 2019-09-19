---
title: algorithm course searching
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:17:17
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## Searching

### BF

```

search(v){

	for a in array:
		if a.value = v:
			return true;
	return false;
}
```





### BST



> 这里研究的关键问题是searching，设为key-value对，那么目标是查找value，一般来说目的是查找该value是否存在，或者更进一步返回位置。



> 由前面分析知，对于searching问题
>
> 1. 组织为原本的形式，BF O(n)
> 2. 组织为hash的形式，O(1)
> 3. 组织为树的结构，O([logn])
>
> 我们知道，搜索的长度实际上与搜索的树的高度密切相关，所以我们希望寻找平衡的树。



> 本身有多种平衡树的结构，我们这里RBT



```
递归定义，由递归定义无论如何是很容易地去写出原本的结构的。

RB0

RB: black (RB/ARB, RB/ARB)

ARB: red (RB, RB)

insert()
delete()

class RBTree
	Element root;
	RBtree leftSubtree；
	RBtree rightSubtree;
	int color; // red,black
	
	static class InsReturn
		public RBtree newTree;
		public int status; //ok,rbr,brb,rrb,brr
```

 



#### Insertion



### Hashing

>这里在Insert使用均摊分析， 即ArrayDouble



```
Insert
Delete //? 删除是否需要收缩
Find

hashingInsert(H, x){
	size = num = 0;
	if size = 0:
		allocate a block of size 1:
		size = 1;
	if num = size:
		allocate a block of size 2size:
		size = 2size;
	insert x into the table;
	num ++;
}


```
