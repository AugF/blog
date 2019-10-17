---
title: algorithm acwing improve class
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-16 14:57:57
tags:
- algorithm
- acwing
- improve
categories:
- [algorithm, acwing, improve]
---

## 1. 概览

1. 算法学习分为这几类：
    - level 1 语法课, 直接题库搜索"语法课"即可进行练习
        > https://www.acwing.com/problem/search/1/?csrfmiddlewaretoken=msUJJ5LtxRvIldBlsslvchYSw8Grn8UdIM32F2WtpNs1WitNTjsvLznTVFof8cW2&search_content=%E8%AF%AD%E6%B3%95%E9%A2%98
    - level 2 算法基础课
    - level 3 算法提高课 算法的应用
        - 题目--> 模型 ---> 相似的题目(因为整理的人太少，所以这里暂时以题目为主) 题谱

2. 算法题考查两部分的内容：
    1. 思维
    2. 写代码的熟练度

3. 非常好的网站: 衡阳七中
    > 看延迟，直接看时间就可以了

## 2. DP

### 2.1 数字三角形

1. basic 
    - 采花生问题
        > 从集合角度来考虑问题, 一个集合就代替了暴搜中的一个元素
        - 状态表示 f[i, j]
            - 集合： 所有从(1, 1)走到(i, j)的路线
            - 属性： Max/Min/数量： 集合中所有集合的每个元素的最大值；
                > 于是f(n, m)就是目标值;  计算该值实际上就是寻找一个拓扑排序
        - 状态计算： 集合划分  分而治之
            > 依据最后一步来划分
            - 划分依据
                - 不重复（最值无所谓，数量必须要）
                - 不漏（所有的都必须考虑）
                > 本题图的连通性
    - 最低通行费： 最大值往往不需要初始化，最小值需要进行考虑

2. improve
    - 方格取数：难点在于如何考虑走两次
        - 走两次： 同时走
            > f[i1, j1, i2, j2]表示所有从(1,1), (1,1)分别走向(i1,j1),(i2,j2)的路径的最大值。 
            - 如何处理“同一个格子不能被重复选择”
                > 只有在i1+j1=i2+j2时，两条路径的格子才可能重合；一开始考虑使用f[i1, j1, i2, j2], 但是发现可以少一维的变量!!!
                > f[k, i1, i2]表示所有从(1,1), (1,1)分别走到(i1, k-i1), (i2, k-i2)的路径的最大值， k表示两条路线当前走到的格子的横纵坐标之和
            - 状态计算： 集合划分=下+下， 下+右， 
                > (1,1)-> (i1-1, j1), (i2-1,j2) -> (i1, j1), (i2, j2)
                - 集合划分
                    > 将(1,1)到(k, i1, i2)的路分为两种
                    > (1,1)--> (i1-1, j1) -> (i1, j1)
                    > (1,1)--> (i2-1, j2) -> (i2, j2)
                    > f(k-1, i1-1, i2-1), 根据(i1,j1),(i2,j2)是否是同一个格子，重合 w(i1, j1); 不重合， w(i1, j1)+w(i2, j2)
                    - 1： 下， 2： 下
                    - 1： 下， 2： 右
                    - 1： 右， 2： 下
                    - 1： 右， 2:  右

### 2.2 最长上升子序列问题

LIS(longest increase subsequence)
    1017
        1014，482
    1012
    1016
    1010(+贪心)
        187(+dfs)
    272(LCS)

#### 2.2.1 basic
895: 最长上升子序列问题
- 状态表示f[i]
    - 集合： 所有以a[i]结尾的严格单调上升子序列
    - 属性： Max
- 状态计算
    - 划分依据：最后一个不同的点。
        > 如果不能在前面很快判断，那么最基本的想法就是枚举来做，空
        
> LLS实际上可以从n^2转化为nlogn, 实际与后面的一步的转化。 优化？

```
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 110;

int n;
int a[N], f[N];


int main()
{
    int T;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d", &n);
        for (int i = 1; i <= n; i ++) scanf("%d", &a[i]);

        // 正向求解LTS问题
        int res = 0;
        for (int i = 1; i <= n; i ++) 
        {
            f[i] = 1;
            for (int j = 1; j < i; j ++) 
                if (a[i] > a[j])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        // 反向求解LTS问题
        for (int i = n; i; i --)
        {
            f[i] = 1;
            for (int j = n; j > i; j --)
                if (a[i] > a[j])
                    f[i] = max(f[i], f[j] + 1);
            res = max(res, f[i]);
        }

        printf("%d\n", res);
    }

    return 0;
}
```

