---
title: daily cplus thinking
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:13:45
tags:
- cplus
- thinking
categories:
- [daily, thinking]
---


## C++ 多文件组织

### 1. 问题


之前大学学习的时候也是这个问题一直在困扰自己没有得到解决。
主要想思考着将计算机系统基础所学知识联系起来思考。
首先，两个问题：
1. 多个cpp中调用同一个.cpp, 或者重复调用一个.h, 或者变量重复怎么办
2. 头文件中有那个#ifdef定义的怎么用

> 现在已经知道的就是c++在编译的过程中是先将cpp文件进行编译（有时间再复习计算机基础的知识，结合起来进行思考）。

### 2. 想法

gcc编译过程有静态链接和动态链接的过程。对于动态链接就是增加已经预先编译好的东西。

一般来说，编译器会做以下几个过程：
1. 预处理阶段
2. 词法与语法分析阶段
3. 编译阶段，首先编译成纯汇编语句，再将之汇编成跟CPU相关的二进制码，生成各个目标文件(obj文件)
4. 连接阶段，将各个目标文件中的各段代码进行绝对地址定位，生成跟特定平台相关的可执行文件

注意，1主要是处理源文件中的宏、变量、函数声明、嵌套的头文件包含等东西，然后重新生成C文件。
2和3通常为一起，此时已经产生了符号表。已经将全局符号和局部变量分开了。而且在操作系统中，建立了对应的符号表以及将定义位置和引用都联系好了。同时，已将将全局变量还有程序转入了对应的程序的位置区段。

哪些已经编译好的内容是指直接只差操作系统指定将代码放在哪里了，只要操作系统指定位置，那么就可以指定按行行动，然后产生文件了？
> 刚刚看了维基百科，其实这里所表述的链接也很正常，易于理解。实际上就是所想的那样的。dll提出的初衷也就是为了复用，比如window窗口部件。直接复用就可以调用同样的功能。

#### 2.1 静态编译

首先，不管是IDE还是命令行，IDE不过是命令行的一个集成，帮你把什么东西都设置快捷了，让你来进行操作。
gcc .cpp .cpp
它会把每个cpp进行编译成.o文件，再由.o文件一起生成可执行文件。
main.cpp一定是在最后。
##### 为什么有.h和.cpp文件？
.h文件一般是放说明的，而.cpp一般是对应的实现；（同名警告！？ 是会给编译过程带来好处吗？）

如果一个.h中声明太多，需要两个.cpp来书写可以吗，如果允许，这样感觉同名潜规则没必要。不允许，编译器规定死了，相关声明只能从相关同名文件中去找。然后，如果其他.cpp声明要用到该环境直接用即可。

所以一般来说，那我们再构建其他文件时，只需要include .h文件就可以了啊，没有必要引入其他文件。

对啊，因为你生成的是cpp文件，所以这里每个cpp对应的是.o， 它会在对应的表上记录。
那么对应的cpp与源文件存在依赖关系，所以这里就涉及到依赖关系。
> 依赖关系，这里就是图的拓扑排序。还有一个问题，记得计算机系统基础中有讲过gcc 后面的参数对表的构建其实也是有影响的

同时，又可以回答一个问题，在cpp中引用stdio.h也是没有问题的啊。因为这些不过是标志着它会去找文件，所以完全就没有关系。最终不过是生成.o文件是，它会把对应的库中的文件进行替换而已。


##### 一些关键字作用的思考
1. incline
    ？incline声明的会先一步静态编译到程序中。
    extern感觉上更多的用法是在一个cpp文件中使用的是另一个文件中的，然后却并不会指出是在哪个文件中定义又初始化的，不安全，不建议使用该关键字。


2. define
    \# define关键字的使用也相当明了了。我们的目的是什么，就是每个.h文件应该管理自己特有的变量和函数声明这样我们就能使程序避免变得杂乱无章。而且像`#define FILE_H`这种操作，只能在file.h中做啊，其他情况你为什么要这么做。而加 
    ```
    #ifndef CPLUS_LAB1_STACK_H
    #define CPLUS_LAB1_STACK_H
    stack.h特有的
    #endif //CPLUS_LAB1_STACK_H
    ```
    不过是避免犯错，编译特有的。

    思考define的第二种用法，首先在命令行或者某个场合环境下，你需要说明你现在是什么环境，或者代码里定义环境。然后，在代码里使用对应的条件判断语句也就很轻易的判断出是哪个了。
##### 多文件组织
多个.h之间的依赖，有什么例子吗？如果是结构体，比如有个queue的操作，现在我又要新建一个优先级queue的操作，所以可以用到queue的操作。那么就涉及到进阶的知识即抽象为类，以及类之间的关系表示。

一般来说书写.h文件的话没有必要那么多和复杂。而且一般是直接引用两个相对独立的.h, 共同使用它们的函数。然后其实一个cpp也可以调用多个.h的，那么这样的话，按前面的想就明了。
有反常情况吗？

还有一个问题可以创建文件的问题，至少在clion中，因为MakeFile是自己书写，所以只用把文件路径点名即可，照样可以操作，对。

#### 2.2 动态链接

.dll未明朗解决


### 3. 进一步思考和建议

MakeFile相关的东西
整理以及实践的结果

一点额外的东西：
一般情况下，程序并不关心栈的具体分配情况，但进行混合语言编程时，则要考虑不同语言在栈使用上的差别。
> 1. 还需要考虑哪些？

### 4. 再理解的内容
- [关于如何将多个Cpp组织起来 -jamesheros](https://blog.csdn.net/u010167037/article/details/19680877)
- [菜鸟攻略-C语言多文件编程初探 -流沙的刺客](https://blog.csdn.net/candcplusplus/article/details/53326368)
- [头文件与同名源文件的关系-奔跑的路](https://blog.csdn.net/lee244868149/article/details/39341751)
- [C语言中，头文件和源文件的关系](https://blog.csdn.net/xhbxhbsq/article/details/78955216)
- 计算机系统基础, 袁春风
- 程序员的自我修养-链接、装载与库
    > 只是被推荐，未读