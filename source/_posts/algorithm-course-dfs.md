---
title: algorithm course dfs
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 08:51:42
tags:
- algorithm
- nju
- course
categories:
- [algorithm, nju-course]
---
## DFS

> 配合笔记使用，这里假设为简单图

```
// 统计变量
struct Vertex{
    string color;
    int parent;
    int dis;
    double discovertime;
    double finishtime;
}

// 处理不同连通分支
DFS-Wrapper()
{// 其中n为数组的长度
    Vertex[]  V = new int[n+1]'
    Edge[] E = new int[m+1];

    // initiliaze
    for v in V:
        v.color = white;
    time = 0;
    
    for v in V:
        if color[v]==white:
            v.parent = -1;
            DFS(v);
}
```





### 有向图

```
DFS-directed(v) {
    v.color=gray;
    time ++;  v.dicovertime = time;
    pre processing of dfs;

    for w that vw in E:
        if w.color = white:
            <pre-processing of TE>
            w.parent = v;
            DFS(w);
            <post-precessing of TE>
        else if w.color = gray:
            // BE
        else if w.color = black:
            if w.parent ->> v:  DE
            if !(root(w,v)) || root(w,v) not in {w,v}: CE

    post-processing of dfs;
    time++; v.finishtime = time;
    v.color = black;
}
```



### 无向图

> 1. 注意这里的TE,BE,DE,CE都是实际上存在的边；如果一个边访问两次，则可以将其看作是二次遍历。
> 2.  再试想一下，如果边标记是否被访问；那么一条边可能二次遍历。如果剔除二次遍历。
> 3.  !!!! 无向图的DFS,  CE不存在



```
DFS-undirected(v) {
    v.color=gray;
    time ++;  v.dicovertime = time;
    pre processing of dfs;

    for w that vw in E:
        if w.color = white: //TE
            <pre-processing of TE>
            w.parent = v;
            DFS(w);
            <post-precessing of TE>
        else if w.color = gray:
            if w.parent != v:  // BE
            if w.parent == v:  // 二次遍历vw
        else if w.color = black:
            if w.parent == v // BE 二次遍历

    post-processing of dfs;
    time++; v.finishtime = time;
    v.color = black;
}
```



## 应用

有向图

1. 拓扑排序

2. Critical Path

3. SCC

无向图

1. 割点

2. 桥
