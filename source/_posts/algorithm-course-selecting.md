---
title: algorithm course selecting
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:18:24
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## Selecting

> Selecting的目标是选择出阶为k的元素，即第k小的元素，注意此时寻找的应该是函数值



> 因为是基于比较的选择，即使是选择max,min都需要(n-1)次的比较，所以下界必然是O(n).
>
> 随着k的增加，很多时候很有可能到达O(n^2)
>
> 所以目标是期望线性时间选择，以及最坏情况线性时间选择



### 期望线性时间选择

> !!!! 这里用到的是快速排序中的partition概念，这很符号第k小元素本身的性质。



```
思想： Partition算法是采用使用第一个元素或者其他元素的方式，得到每个划分值。

search(A,l,r,k)步骤：
0. 如果l==r, 返回
1. 使用Partition算法得到q, x=q-l+1
2. 如果k大于x，则search(A,q+1,r,k-x+1)；如果k等于x，return A[q]; 如果k小于x， 则search(A,l,q-1,k);

search(A,l,r,k){
	if (l==r) return A[l];
	
	q = Partition(A,l,r);
	x = q - l + 1;
	
	if(k == x) return A[q];
	if(k > x)
		search(A,q+1,r,k-x+1)
	if(k < x)
		search(A,l,q-1,k)
}
```



### 最坏情况线性选择

> 这里的改进点主要在于是q尽量在中位数附近。
>
> 使用我们之前所学到的哪些知识。

最坏情况线性选择



```
m* = select_median(A, l, r)

select_median(A, l, r){
	n = l - r;
	if(n<=5) solve_median(A,l,r);
	else {
		cnt = floor(n/5);
		int[] B=new int[cnt+1];
		for i in {1,..,cnt+1}:
			if i == cnt+1: 
				B[i-1]=select_median(A, l+5cnt, r)
			else
				B[i-1]=select_median(A, l+5(i-1), l+5i);
		return select_median(B);
	}
}
```
