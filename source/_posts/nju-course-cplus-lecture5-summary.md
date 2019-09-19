---
title: nju-course-cplus-lecture5-summary
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:54:53
tags:
- nju
- course
- cplus
categories:
- [course, nju, cplus]
---
总结

# 输入输出
按输入输出的类型分为三种：控制台、文件、字符串（局部的内存区域）
1. 控制台
- C
    - 输出
    > putchar(), puts(), printf()  %d,c,u,f
    - 输入
    > getchar(), gets(), scanf()
- C++
cin,  cout, cerr直接, clog
iomanip:   setw()?   setiosflags(scientific, flags) setprecision
cout.write();
cin.get(), cin.read(); 
cin.fail()  // 是否输入成功，0表示成功
> 一般地，对输出不需要判断文件结束，输入需要判断文件结束
- 操作符的重载： 继承的问题， display()

2. 文件
永久保存数据的设备
文本(字符串)，二进制文件(机器表示) 
- C
    - 输出
    fopen(), fputc(), fputs(), fprintf(),size_t fwrite(sizeof()) //字节，二进制, fclose()
    > 目录问题，路径写法。  打开方式: w创建,清除,  a尾  b; 不同操作系统，windows \n->\t\n
    - 输入
    fopen() NULL失败，
    > r, b,  fgetc(), fgets(), fscanf(), fread(sizeof()). feof()判断是否到的文件的末尾
    - r+, w+, a+,  b
    - fseek(), ftell, cur,end,set
- C++
输出
out_file(), open, write, <<
输入
in_file(),read,  eof

3. 字符串
char buf[100];
ostrstream str_buf(buf,100);

4. 其他
traits

> 思考，为什么独独设置了这三类？

# 异常处理

所谓异常，其实就是程序过程中出现的bug。其实主要可以分为以下几种。
1. 语法错误， 编译器会解决
2. 逻辑错误，很关键，要学会排错
- 不能运行，实现的逻辑有问题
- 能运行，有问题
    - 输出错误
        - 程序逻辑错误
        - 有的函数的输入输出没有检查
    - 编译出错
        - 平台相关
3. 运行异常
    - 程序对运行环境考虑不周
    > 这里很重要，务必注意程序的环境

鲁棒性，就是能应对足够多的异常。

处理异常的方法：
直接，当地抛出异常，即exit();
最大的问题可能就是当程序特别大的时候，程序员不知道什么地方出错了。

异地处理：参数值，新建参数值，全局变量，这些都不好。

推出抽象化，实际上也是通过全局变量来做的，不过交给了底层。上层可以用最简单的语句进行实现。

try{
    检测语句；
}catch(类型 变量)

throw{}
一旦进行检测必然抛出，很优秀。
而且只抛出子层的，向上处理。没有就控制台。

上面是无法避免的可能出现的运行异常的处理。

只要不可控都要进行上面的处理。

下面聊到代码调试，如何测试bug

一般的怎么做就分析输出语句呗，看输出语句是否正确。
一种抽象assert();
易于定义位置。
以及可以一键清除。
```
#define NDEBUG
#include<cassert>
`