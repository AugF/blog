---
title: nju-course-cplus-lecture4-summary
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:52:19
tags:
categories:
---
泛型程序设计

> 类型的多态

首次尝试，c使用泛型指针。c++template关键字，剩余的事情交给编译器。
代码写得简单，就可以当做常见的int来做，而且不容易出错。
封装了一层。

what where who when how
1. 哪里使用？
普通类型，直接，在类中。
特殊类型——类，需要把对应的操作，构造函数，复制构造函数处理安全。

2. 怎么用？
template<class T,int size> 可加非这个量。
不具体化，不认的。

类型冲突的请显式说出来

注意了：实现必须写在.h中，这是代码复用；不用担心重复解释，编译器会解决；然后，全局函数其他都要把<class T>标出来，特别是友元。

3. 于是，C++, STL
远离类型，就是实际所分析和学的数据结构和算法，所以妙啊！
不过遇到具体类型套而已！
- 容器，常用数据结构。
> 这里是序列化数据; 复杂的数据结构可以由这些给组合而成的，哈哈哈！

- 算法
> 一些最常用的操作集合

- 迭代器
> 也就是算法中对数据结构的抽象吧，主要是抽象为数组的处理一样，可以轻易找到头尾，下一个元素，以及向前，向后。

[迭代器是一种检查容器内元素并遍历元素的数据类型](https://blog.csdn.net/liyuan_669/article/details/22100165)

字面不要过度推理，从权限上看

InputIterator: 单步迭代，不允许修改
相当于`vector<int>::const_iterator iter, 可以iter++, 不可以*iter=1;`注意区分
`const vector<int>::iterator iter=v.beign, 可以*iter=1,不可以iter++` 

OutputIterator: 单步迭代，可以修改。

> 突然这里对前面返回两种不同的东西有了实质的感觉，一个是正常，另一个是既不可以被修改，也不可以增加

0: queue,stack,pq
InputI:
OutputI:
ForwardI:
BidrectionalI: list,map,set
Random-accessI: vector,deque,basic_string

类型
- 添加 push, insert
- 大小 size, empty , resize,count
- 访问 back,front, [],at,top,find
- 删除 erase,clear,pop_back
- 赋值与swap: =,swap, assign()//重新设置

> 然后按照需要思考会用哪些？

> 循环遍历
```
  for (it=phone_book.begin(); it != phone_book.end(); it++) 
// 注意这里 end()返回的是最后的下一个元素，按照链表来看就是null. 具体是什么值我也不知道
```
> 首尾元素是head, tail

4. todo
再想不同迭代器

一种类型的定义：[如果需要定义真正的tuple，那就需要用const vector<int> nums(10,9); 然后用const_iterator来访问](https://blog.csdn.net/zhanh1218/article/details/33340959)


C++11新增加的内容
```
auto i1 = Container.begin();  // i1 is Container<T>::iterator 
auto i2 = Container.cbegin(); // i2 is Container<T>::const_iterator
```

所以cbegin()的访问控制就是public

> 不同迭代器代码上怎么看？怎么区分。对于容器来说是都返回其基类？由具体用的时候，再判断和出错.

[褚略理解](https://blog.csdn.net/sim_szm/article/details/8980879)
- 输入。  find, accumulate;  标准库istream_iterator
- 输出。 copy, ostream_iterator
- 前向. replace
- 双向。 reverse
- 随机访问 sort, <, >, !=, +,-,回退多个元素
> 具体的算法有具体的应用

> 发现很多接口的改进点：增加了越界的判断。比如[]和at，细想真的牛逼！


5. 算法

分类
- 调序算法：实现按某个要求改变容器中元素次序的操作
> 比如sort, 

- 编辑算法：实现对容器元素的复制、替换、删除、赋值等操作
> 比如replace, transform

- 查找算法：实现在容器内查找元素或子元素序列等操作
> find,count_if

- 算术算法：实现在容器内进行求和、内积和、差等操作
> accumulate

- 集合算法：实现集合的基本运算，要求已经排好序？

- 堆算法：实现基于堆结构的容器元素，第一个元素最大
- 元素遍历算法 for_each

算法指定在迭代器上，妙！迭代器自身相容，妙！
规定，作用范围一样，src_first,src_last, dst_first

函数参数：进行自定义，类型也为T

一个超级有趣的例子。

匹配一定条件的东西， count_if()->类->lambda表达