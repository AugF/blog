---
title: algorithm course Greedy
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:01:25
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## Greedy

> Greedy就是BestFS

Greedy 满足三个条件

1. feasible, 即满足约束

2. Locally optimal
3. Irrevocable: 不可回退



```

// 过程可以看作是不断从candidate中，根据某种策略选择local optimal, 然后更新candidate, 重复选择过程，知道candidate为空，或者已经找到问题的解

step by step解决问题

Greedy(candidate) {
	S = null;  // S为解决问题的解集

	while not solution(S) and candidate != null: {
		choose locally optimal x from candidate;
        
        candidate = candidate - {x};
        if feasible(x) then S = S + {x};
	}
		
	if solution(S) return S;
	else return "no solution";
}
```



## MST

```
primMST(G,n) {

	initialize the priority queue pq as empty;
	Select vertex s to start the tree;
	Set its candidate edge to (-1, s, 0);
	
	insert(pq, s, 0)
	while(pq is not empty){	 
         v = getMin(pq); deleteMin(pq);
         add the candidate edge of v to the tree;
         updateFringe(pq, G, v)
	}
	return
}

updateFringe(pq,G,v){
	for w that vw in E: //2m loops?
		newWgt = w(v,w)
		if w.status is unseen then
			Set its candidate edge to (v,w newWgt)
			insert(pq,w,newWgt)
		else // w in Fringe, 需要更新在pq中的权值
			if newWgt < getPriorty(pq,w)
				Revise its candidate edge to (v,newWgt) // 记录哪个边才是真正与之相邻
				decreseKey(pq, w, newWgt) // 改变pq中w的权值
}

?--priorityqueue 
- getMin(pq)
- deleteMin(pq)
- insert(pq, w, newWgt)
- decreseKey(pq, w, newWgt)
```



### BestFS

```
Initialize the priority queue Fringe as empty
while Fringe != empty do
	v: = Fringe.extract-min();
	<process-v>
	update-fringe(v, Fringe)

subroutine update-fringe(v, Fringe)
	for w of v which is Frensh do
		set the priority of w, insert w to Fringe
	for w of v which is Fringe do
		update the priority if w in Fringe, if necessary.
```

