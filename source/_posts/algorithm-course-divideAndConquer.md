---
title: algorithm divideAndConquer
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:54:33
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---

## DivideAndConquer



### 1. Recursion



> 首先，BF数量级别就是n, n-1, n-2, ....
>
> DivideAndConquer数量级别是n, n/2, n/4, ...
>
> 本质上，Recursion就是循环，不过不同的就是需要用辅助数组加for循环的结构来替换Recursion。

```
Recursion(a, b)
{
	if a == 1:
		return 1;
	return Recursion(a/2, b);
}

max_size = log_2 a;  // 递归的次数
C[max_size+1]; //用于每次递归的结果的存储
C[1] = 1;
for i in (1, maxsize+1);
	C[2^{i}] = C[2^{i-1}]
```



### 2. DivideAndConquer

```
The general pattern

directlySolve(I){
	return ans;
}

solve(I)
	n = size(I)
	if (n <= smallSize)
		solution = directlySolve(I)
	else
		divide I into I_1, I_2,.., I_k;
		for i in {1,..,k}:
			S_i = solve(I_i);
		solution = combine(S_1, ..., S_k)
	return solution
```

Max-Sum

```

```
