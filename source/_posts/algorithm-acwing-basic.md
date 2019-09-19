---
title: algorithm acwing basic
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:15:53
tags:
- acwing
- algorithm
- basic
categories:
- [algorithm, acwing, basic, basic]
---

## 1. 快排
```
#include<iostream>
#incldue<stdio.h>
using namespace std;
const int N=1e6+10;
int n;
int q[N];

void swap(int &x,int &y){
    int t=x;
    x=y;
    y=t;
}
void quick_sort(int q[], int l, int r){
    if(l>=r) return;  // 注意边界条件不可少

    int x=q[l], i=l-1, j=r+1; // 或q[(1+r)/2], q[r]
    // 这里赋初值是为了后面对称的情况
    while(i<j) {
        do i++; while(q[i]<x);
        do j--; while(q[i]>x);
        if(i<j) swap(q[i],q[j]); // 注意这里要加一个判断
    }

    quick_sort(q, l, j);  // 
    quick_sort(q, j+1, r);
}

int main(){
    scanf("%d", &n);
    for(int i=0;i<n;i++) scanf("%d", &q[i]);
    quick_sort(q,0,n-1);
    for(int i=0;i<n;i++) printf("%d", q[i));
}
```

## 2. 归并
```
#include<iostream>
using namespace std;
const int N=1e6+10;
int n;
int q[N], tmp[N];

void merge_sort(int q[],int l, int r){
    if(l>=r) return;
    int mid=l+r>>1;
    merge(q, l, mid);
    merge(q, mid+1,r);

    int k=0, i=l, j=mid+1;
    while(i<=mid&&j<=r){
        if(q[i]<q[j]) tmp[k++]=q[i++];
        else tmp[k++]=q[j++];
    }
    while(i<=mid) tmp[k++]=q[i++];
    while(j<=r) tmp[k++]=q[j++];

    for(i=l,j=0;i<=r;i++,j++) q[i]=tmp[j];
}

int main(){
    scanf("%d",&n);
    for(int i=0;i<n;i++) scanf("%d",&q[i]);
    merge_sort(q,0,n-1);
    for(int i=0;i<n;i++) printf("%d", q[i]);
}

```

## 3. 二分

### 整数二分
// 整数二分，有单调性一定可以二分；但是没有单调性也可以二分
// 在区间上定义上某种性质，将区间分为两部分，左边满足某种性质，右边不满足某种性质；二分法可以用来寻找不满足性质和满足性质的边界
？

```
int bsearch_1(int l,int r){
    while(l<r){
        int mid=l+r>>1; // 下取整
        if(check(mid)) r=mid; // [l,mid]
        else l=mid+1;
    }
    return l;
}

int bsearch_2(int l, int r){
    while(l<r){
        int mid=l+r+1>>1;
        if(check(mid)) l=mid;
        else r=mid-1;
    }
    return l;
}

如何判断死循环
mid=l=r时，是否陷入死循环
```

整数二分的问题一定可以用这个模板来解决。
每次二分，能找到一个答案的区间；当区间的长度是1时，即是解

二分能够包含解，然后用边界可以推出解。
> 二分的退出条件一定是i=j吗？，这里知道快排的退出条件并不是l=r
对！！！！，二分一定能找到解，退出是i一定等于j

> 性质的寻找， 如果a[mid] < x, 怎么办？实际上就是看目标的数在a[mid]与x之间，比如靠近x最大的数， 此时l=mid; 如果是找x相等的左边界，在a[mid]
在a[mid] > x, 比如
> ??? 有点模糊

### 浮点数二分simple
> 这里就不用+1，-1
求平方根

> 这里务必注意左右范围的选取问题。

```
#include<iostream>
using namespace std;
int main(){
    double x;
    cin>>x;

    double l=0, r=x; // r=max(1,x);
    while(r-l>1e-8){  // 这里比要求的有效数字多2
    // 这里用迭代次数来停止也可以，比如100，即除以2^100.
        double mid=(1+r)/2;
        if(mid*mid >= x) r=mid;
        else l=mid;
    }

    printf("%lf\n",l); 
    // 这里l或者r
    return 0;
}
```

## 4. 高精度
A+B： len(A)=1e6
A-B: 
A*a 1e6  a-10000
A/a 

存储，倒序存储
在末位加上一位，下标为0的存个位。
```
A+B
int a[N],b[N],sum[N];
int cnt=0;
for(int i=0,j=0;i<n&&j<m;i++,j++){
    sum[i]=(a[i]+b[j]+cnt)%10;
    cnt=(a[i]+b[j]+cnt)>10?1:0;
}
if(cnt) a[n+1]=1;

A-B
len(A)>=len(B)
```

如果A，B可能取正或负，那么一定可以转化为绝对值进行相加或者相减！


高精度减法
```
t=(t + A[i])/b;
c.push_back(t%10);  //?
t=A[i]%b;

for (int i=A.size()-1;i>=0;i--)
t=10t+A[i];
if((t/b)) c.push_back(t/b);  
t %= b;
t
```

## 5. 前缀和和差分
- 前缀和
a1, a2, a3,..., an
S0, S1, S2,..., Sn
a_{i}= S_{i}- S_{i-1}
S0=0;
当S0=0时，特别好定义边界。
前缀和 Si = Si-S0. 

因此，可以很容易地求片段和。
> 很厉害！可以统一了，不用预处理了

> scanf比cin快一倍
> ios::sync_with_stdio(false); 
> cin和标准输入输出不同步，提高cin读取速度；不能使用scanf

区间和
区域和
(x1,y1), (x2,y2)
Sx2y2-Sx1y2-Sx1y1+Sx1y1

- 差分
前缀和的逆运算
> 作用：可以在O(n)时间内B->A
区间[1,r]+c
> b[l]+c, b[r+1]-c

## 6. 双指针算法
两类： 一个数组两个指针；两个数组同时处理。
代码框架
```
for(i=0,j=0;i<n;i++){
    while(j<i && check(i,j)) j++;
    res=max(res, j-i+1);
    // 每道题的具体逻辑
}
O(n+m) 
这里i和j具有单调性，i和j只向一个方向发展。
```

核心思想
```
for(int i=0;i<n;i++)
    for(int j=0;j<n;j++)
        O(n^2)
```
将上面朴素算法优化到O(n), 运用了某种性质

维护两个窗口

```
ad daa
ad
daa
#include<iostream>
#include<string.h>
using namespace std;

int main(){
    char str[100];
    gets(str);
    int n=strlen(str);
    
    for(int i=0;i<n;i++){
        int j=i;
        while(j<n && str[j]!=' ') j++; // i,j-1具体单词
        for(int k=i;k<j;k++){
            cout<<str[k];
        }
        cout<<endl;
        i=j;
    }
    
    return 0;
}
```

```
// 枚举起点和终点
// 每个指针的位置都有意义
 for(int i=0;i<n;i++)
    for(int j=0;j<n;j++)
        if(check(i,j)){
            res=max(res,i-j+1);
        }

// 双指针算法 O(n)
for(i=0,j=0;i<n;i++){
    while(j<i && check(i,j)) j++;
    res=max(res, j-i+1);
    // 每道题的具体逻辑
}
```
 特殊情况的举例

 > 数组元素的目标和为什么限制为一个解，因为如果是多个解时。对于
 `1 1 1 1 1, 1 1 1 1 , 2`本身就要O(n*m),算法必然为O(n^2)的。
 > ????
## 7. 位运算
 
从个位开始算,(11111)_2
> 先把第k位移到最后一位
> n>>k & 1 看一下n的第k位是几？

```
    for(int k=3;k>=0;k--)
        cout<<(n>>k&1)<<endl;
```
lowbit(x): 返回x的最后一位1
x&(~x+1)= x&-x

## 8. 离散化，整数的离散化
10^9 稀疏的值域，映射到从0开始的自然数 10^5
注意这里映射的话的思想实际上就是按照个数索引来映射的感觉。

a->b
> 1. a[]可能中有重复元素：  去重
> 2. 如何算出a[i]离散化后的值：a是有序的，二分 

```
vector<int> alls;
sort(alls.begin(), alls.end());
alls.erase(unique(alls.begin(), alls.end()),alls.end()) // 去重
> ?这里unique返回的就是改变后的alls, 且alls后面的重复的元素都放在最后部分

// 二分求出x对应的离散
int find(int x){ // 找第一个大于等于x的位置
    int l=0, r=all.size()-1;
    while(l<r){
        int mid=l+r>>1;
        if(x<=all[mid]) r=mid;
        else l=mid+1;
    }
    return r+1; // 这里是否加1都无所谓，这里从1开始映射；1,2,3,4,..,n
}
```

注意，一般的离散化的目标就是所有会用到的坐标
区间和 n+2m(l,r)

## 9. 合并区间

1. 按区间做变短排序
2. 总共有三种情况
`ri<ri+1, ri+1<ri, ri<li+1`
一般是贪心，优先对左右端点进行排序

## 其他
java输入输出比较慢

编译语言和即时编译
小数据 javascript,python快
大数据 go,c,c++快
javascript>python>go=C++>java

### 错误类型
- Segmentation Fault
基本情况是否没有考虑
数组是否越界
没有输入


- Time Limit Exceeded   
输入输出可能都存在着问题


关键：把自己想的算法能够实现出来


