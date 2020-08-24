---
title: errors in c++
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-11-03 10:56:04
tags:
categories:
---

## bugs
1. Segmentation Falut
删代码法, 段错误，溢出； 就是for循环没写对的问题
深搜里面爆栈了，进入了死循环

2. Complic Error


3. Memory Limit Exceeded
内存超限
    > 发现是i < a.size() || b.size() 这里的操作出错

图论问题TLE很大的一个问题可能就是邻接表忘记初始化了memset(h), 还有一个就是无向图M双倍，不要忘了

4. 结果是多个一模一样的值，多半是数组越界的问题。就是输入数据的问题

5. Float Point Exception
表示除0错误

## 其他
为什么用static?
防止每次调用函数时重新分配内存，效率会更高

https://zhuanlan.zhihu.com/p/57512786


如果对于邻接链表的结构出现 Time Limited
检查h[N]忘记了初始化为-1

如果大部分数据已经过了，1,2个数据没过，说明边界问题没处理好
## else

csp考试准备

1. 编译环境准备
2. 时间熟悉，考场熟悉
3. 题型熟悉
4. 考试资料熟悉
    1. stl, 算法
    2. acwing复习课

重定向
```
#include <iostream>
#include <fstream> 
#include <cstring>

using namespace std;

int n, m;
int w[10][10];

int main() {
	ifstream in("in.txt");
	ofstream out("out.txt");
	streambuf *cinbuf = cin.rdbuf();
	streambuf *coutbuf = cout.rdbuf();
	
	cin.rdbuf(in.rdbuf());
	cout.rdbuf(out.rdbuf());
	
	cin >> n >> m;
	
	for (int i = 1; i <= n; i ++)
		for (int j = 1; j <= m; j ++)
			cin >> w[i][j];
			
	for (int i = 1; i <= n; i ++) {
		for (int j = 1; j <= m; j ++)
			cout << w[i][j] << ' ';
		cout << endl;
	}
	
	cin.rdbuf(cinbuf);
	cout.rdbuf(coutbuf);
	return 0;
}
```

## c++ 新特性

dev c++ 5.4.0

tools -> complier options -> add following commands when calling the complier: -std=c++11

新特性
for (auto x: nums) cout << x << endl;

设置autosave


```
str常见操作

string -> char[]
char *c;
string s = "1234";
c = s.c_str(); // c_str()返回的是指针

char c[20];
string s "1234";
strcpy(c, s.c_str());

char[] -> str
char b[8]={'a', 'b'};
string str(b); //()一般是初始化

str += substr
```

注意除了，初始化不能进行{}赋值

c++: 
replace(start_pos, len, replace_str) // 指定长度
replce(line.beigin(), line.begin()+6, replace_str); // 迭代器从开始到结束


特别地，需要注意关于字符串这部分的内容

思考下stdlib.h下有没有好用的一些东西
```
int atoi(const char *str) 将字符串转换为整数
double atof(const char *str)
```

刷题经常用的库
algorithm
- swap, sort, count(f,f+n,value), reverse()

cstring
stoi, stof

int.to_string