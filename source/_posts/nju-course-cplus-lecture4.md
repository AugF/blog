---
title: nju-course-cplus-lecture4
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:51:16
tags:
- nju
- course
- cplus
categories:
- [course, nju, cplus]
---
# 重点

## 泛型程序设计

一个程序实体能对多种类型的数据进行操作或描述的特性称为类属性。具有类属性的程序实体通常有：
1. 类属函数
2. 类属类

类属也是一种多态，称为参数化多态。

比如说用通用指针。

如上所示，C++提供了两种实现类属函数的机制
- 采用通用指针类型的参数(C语言的做法)
- 函数模板

### 通用指针
整个世界就是由1构成的，可以升为万物的。
void *，所有的父类。通用的都是byte.

这些东西然后不断地转换为其他的内容。

```
 函数定义：
 void sort(void *base, unsigned int count, unsigned  int element_size, int (*cmp)(const void*, const void*)){
     // 这里函数参数和也可以诶，发现我完全想复杂了。也就是说这里的话，只要弄个函数名做参数，其实也不所谓，没有我想的那么复杂，哈哈哈！两步就可以了
     // 取第i个元素
     (byte *)base + i*element_size

    // 比较第i个和第j个元素的大小，利用调用者提供的回调函数cmp实现
    (*cmp)((byte *)base + i*element_size, (byte*)base + j*element_size)

    // 交换第i个和第j个元素
    byte *p1 = (byte *)base + i*element_size;
    byte *p2 = (byte *)base + j*element_size;
    for(int k=0;k < element_size;k++){
        byte temp=p1[k]; p1[k]=p2[k]; p2[k]=temp;
    }
 }

 这里的byte为typedef unsigned char byte;

void sort(void *base, //需排序的数据（数组）首地址
             unsigned int count, //数据元素的个数
             unsigned int element_size, //一个数据元素所需的空间大小
             int (*cmp)(const void *, const void *) ) //比较两个元素的函数
{   //不论采用何种排序算法，一般都需要对数组进行以下操作：	
     //取第i个元素
        (char *)base+i*element_size
     //比较第i个和第j个元素的大小 （利用调用者提供的回调函数cmp实现）
        (*cmp)((char *)base+i*element_size,
                    (char *)base+j*element_size)
     //交换第i个和第j个元素
        char *p1=(char *)base+i*element_size,
	     *p2=(char *)base+j*element_size;
        for (int k=0; k<element_size; k++)
        {	char temp=p1[k]; p1[k] = p2[k]; p2[k] = temp;
        } 
} //上面的char用byte替代更好！typedef unsigned char byte;
```

调用者需要调用
```
int A_compare(const void* p1, const void* v2){
    if(*(*A)p1 < *(A*)p2) return -1;
    else if(*(*A)p1) > *(A*)p2) return 1;
    else return 0;
}

// 类属排序函数的使用
int a[100];
sort(a,100,sizeof(int),int_compare);
double b[200];
sort(b,200,sizeof(double), double_compare);
A c[300]; // 如果这里A是类，就显示出了将比较符号重载的有效性
sort(c,300,sizeof(A),A_compare)
```
品味！

### 函数模板
函数模板是指带有类型参数的函数定义，其一般格式如下：
```
template<class T1, class T2,...>
<返回值类型> <函数名>(<参数表>){}
```
```
template<class T>
void sort(T elements[], unsigned int count){
    // 取第i个元素
    elements[i]
    // 比较第i个和第j个元素的大小
    elements[i]< elemnets[j]
    // 交换第i个和第j个元素
    T temp = elements[i];
    elements[i] = elements[j];
    elements[j] = temp;
}

> 来一起感叹，太他妈牛逼了。这template省去了好多麻烦！好高层的一种抽象，太棒了，果然！牛逼死了

int a[100];
sort(a, 100);
double b[200];
sort(b,200);
A c[300];
sort(c, 300);
```

函数模板的使用——实例化
- 函数模板定义了一系列重载的函数
- 要使用函数模板所定义的函数，称为模板函数，首先必须对函数模板进行实例化：给模板参数提供一个类型作为值，从而生成具体的函数。
- 函数模板的实例化通常是隐式的
    - 由编译程序来做，模板实参推演

当编译程序无法根据调用时的参数类型来确定所调用的模板参数，这时，需要在程序中显式第实例化函数模板。

除了类型参数外，函数模板也可以带有非类型参数，使用时需要显式实例化。
template<class T, int size>
void f(T a){
    T temp[size];
}

### 类模板

如果一个类的成员的类型可变，则该类称为类属类。在C++中，类属类用类模板实现。
template<class T1, class T2>
class <类名>{}

使用
Stack<int> st1;

!!! 类模板中的静态成员仅属于实例化后的类，不同类模板实例之间不共享类模板中的静态成员。
template <class T>
class A{
    static int x;
}

注意！在自己定义时，需要把模板的定义和实现都放在头文件中。
```
// file1.h
template <class T> 
class S //类模板s的定义
{  T a;
  public:
    void f();
};
template <class T> 
void S<T>::f() //类模板s的实现  如果放在对应的file1.cpp中，会显示该实例不存在
{ ......
}
extern void func();

```
使用者通过包含这个头文件，把模板的源代码全包含进来，以备实例化所需。
因此，模板是基于源代码的复用、

- 重复实例的处理
模板的复用会导致多模块的编译结果中存在相同的实例：
1. 相同的函数模板实例
2. 相同的类模板成员函数实例
相同代码的存在会造成目标代码庞大！
如何处理？
- 由开发环境来解决：编译第二个模块的时候不生成这个函数
- 由连接程序来解决：舍弃多余的那一个

```
关于类模板的友元
template <class T> class A;
template <class T> void f3(A<T>& a) { ... }
template class<T>
class A
{ T x,y;
  ......
  friend void f1(A<T>& a); //友元f1不是模板！
  template <class T1> friend void f2(A<T1>& a); //f2与A多对多实例化
  friend void f3<T>(A<T>& a); //f3与A一对一实例化(用相同参数类型)
};
void f1(A<int>& a) { ... }
template <class T> void f2(A<T>& a) {...}
......
A<int> a1; //实例化A<int>
A<double> a2; //实例化A<double>
f1(a1); //OK，调用f1(A<int>&)
f1(a2); //链接错误! 调用f1(A<double>&)，但它不存在！
f2(a1); //实例f2<int>是A<int>和A<double>的友元
f2(a2); //实例f2<double>是A<int>和A<double>的友元
f3(a1); //实例f3<int>是A<int>的友元，但不是A<double>的友元！
f3(a2); //实例f3<double>是A<double>的友元，但不是A<int>的友元！

```

> 总结：友元是外部，在说明友元时也需要在外部进行说明。

### C++标准库
在C++标准库中，除了C标准库保留下来的一些功能外，其它功能大都以模板形式给出，这些模板构成了C++的标准模板库。
STL实现了数据结构和算法的复用，体现了泛型程序设计的精髓。
STL支持了一种编程思想模式。

STL主要包含：
- 容器类模板：用于存储序列化数据元素，比如：向量、队列、栈、集合等
- 算法（函数）模板：用于对容器中数据元素进行一些常用的操作，如：排序、查找、求和等
- 迭代器类模板：实现了抽象的指针功能，它用于指向容器中的元素。
    - 迭代器是容器和算法之间的桥梁：传给算法的不是容器，而是指向容器中元素的迭代器，算法通过迭代器实现对容器中数据元素的访问和遍历。

```
#include<iostream>
#include<vector>
#include<algorithm>
#include<numeric>
using namespace std;
int main(){
    vector<int> v;
    int x;
    cin>>x;
    while(x>0){
        v.push_back(x);
        cin>>x;
    }
    vector<int>::iterator it1=v.begin();
    vector<int>::iterator it2=v.end();
    cout<<"Max= "<<*,ax_element(it1, it2)<<endl;
    cout<"Sum="<<accumulate(it1, it2, 0)<<endl;
    sort(it1, it2);

    cout<<"Sorted result is:\n";
    for_each(it1, it2,[](int x){cout<<x<<''; return;})
    cout<<"\n";
    return 0;
}
```

#### 容器

容器是由长度可变的同类型元素所构成的序列。
STL中包含了很多种容器，虽然这些容器提供了很多相同的操作，但采用了不同的内部实现方法，所以不同的容器适合不同的应用场合。
容器由类模板来实现

- vector<>
需要快速定位任意位置上的元素以及主要在元素序列的尾部增加、删除元素的场合
在头文件vector中定义，用动态数组实现

- list<>
用于经常在元素序列中任意位置上插入或删除元素的场合。 
list, 双向链表实现

- dequeue<>
用于主要在元素序列的两端增加、删除元素以及需要快速定位任意位置上的元素的场合
dequeue, 用分段的连续空间结构实现

- stack<>
用于仅在元素序列的尾部增加、删除元素的场合
基于deque实现

- queue<>
用于仅在元素序列的头部增加、删除元素的场合
基于dequeue实现

- priority_queue<>
与queue的操作类似，不同之处在于：每次增加，删除元素之后，它将元素位置进行调整，使得头部元素总是最大的。每次删除操作总是把最大的元素去掉。
在queue中定义，一般基于vector和heap实现

- map<关键字类型，值类型>
multimap
 - 容器中每个元素是一个pair结构类型，该结构有两个成员：first和second，关键字对应first, 值对应second,元素是根据其关键字排序的。用于需要根据关键字来访问元素的场合
 - 对于map，不同元素的关键字不能相同；对于multimap,不同元素的关键字可以相同
 - map中定义

- set<元素类型> 
multiset
 - 相对于map和multimap, 合一了
 - set中定义

- basic_string<>
 - 与vector类似，不同之处在于其元素作为字符类型，并提供了一系列与字符串相关的操作
 - string和wstring分别是它的两个实例： basic_string<char>和basic_string<wchar_t>
 - 在string中定义

#### 容器的操作

- 获取指定位置的元素
- 增加元素
- 删除元素
- 查找元素
- 获取容器首、尾元素的迭代器

```
T &front();
T &back();  // vector, list,dequeue,queue

void push_front(const T&x) void pop_front;  //list,deque
void push_back(const T&x) void pop_back; //vector,list,dequeue

void push(const T&x) // 尾部stack, queue, priority_queue
void pop() // 尾部stack, 头部queue, priority_queue

T &top();  const T&top() const;  尾部stack , 头部priority_queue

iterator begin();
const_iterator begin() const;  除queue,priority queue, stack
// 为什么有两种？有啥不一样的用的地方吗？

iterator end();
const_iterator end() const; 同上

iterator insert(iterator pos, const T& x);
void insert(iterator pos, InputIt first, InputIt last);
// pos 插入一个呼呼多个元素， vector, list, deque

iterator erase(iterator pos)
iterator erase(iterator first, iterator last);
// vector, list, deque, map/multimap, set, basic_string

T &operator[](size_type pos)
// pos上的引用， vector, deque, basic_string

ValueType & operator[](const KeyType& key); // map

T& at(size_type pos);
相比于[], 进行了越界检查； vector,deque,basic_string

iterator find(const T& key);
// 根据关键字查找，找到返回；否则返回最后一个元素的下一个位置
```

> 值得注意的是，如果容器的元素类型是一个类，则针对该类需要：
> 1. 自定义拷贝构造函数和赋值操作符重载函数。
> 2. 重载比较操作符<

#### 迭代器
迭代器是实现了抽象的指针（智能指针）。它们指向容器中的元素。
> 哇，抽象指针，牛逼

在STL中，迭代器是作为类模板来实现的，它们可以分为以下几种：
- 输出迭代器output iterator
    - 用来修改它所指向的容器元素
    - 间接访问操作 *输出迭代器
    - ++操作

- 输入迭代器input iterator
    - 用于读取它指向的容器元素
    - 间接访问操作 *p
    - 元素成员间接访问->
    - ++, ==, !=操作


- 前向迭代器
> 哇，这个就应该是方向有关吧。只能一个方向，不过好奇的是这个方向有规定只能从头到尾吗？感觉上应该两种都被允许
用于读取、修改它所指向的容器元素
元素间间接访问操作和元素成员间接访问.
同义获取关联的对象
> 如果是int，则访问的是其值，就对应于第一种情况；如果是类，就对应于第二种情况。
++,==,!=操作


- 双向迭代器
用于读取，修改它所指向的容器元素
元素间接访问操作和元素成员间接访问操作
++,--,==,!=

- 随机访问迭代器
用于读取、修改它所要指向的容器元素
元素间接访问操作、元素成员间接访问和随机访问元素操作
++,--,>=,<,>
> 随机访问吗？应该是对于固定存储的吧？不然实现算法复杂度不一样唉，真好奇实现方法。
> 对！下面得到了回答

从上到下，从派生类到基类

哦，来啦

**各容器的迭代器类型**
- vector,deque, basic_string,  begin/end返回的是随机访问迭代器
- list,map,set返回的是双向迭代器
> 这两个因为内部的物理实现结构不一样
- 对于queue,stack,priority_queue,不支持迭代器
> 为啥，不支持前向后向吗？queue本身支持，但是因为queue,stack有其特定的持有顺序所以不支持吗？特殊功能，而priority_queue更是由于实现复杂。每一次动荡代价不一样。

> 迭代器的作用就是每个变量的指示，最多的应用自己所见到的就是遍历。所以怎么说感觉上应该能得到下一个元素的地址的那种

#### 算法
分类
调序算法
编辑算法
查找算法
算术算法
集合算法
堆算法
元素遍历算法

STL算法体现了一种高度抽象：
- 每一个算法都完成一个特定的功能
- 大部分算法都是遍历指定的容器元素，对满足条件的元素执行默认的或自定义的操作。
- 使用者只需要提供：容器（迭代器）、操作条件以及自定义操作，而控制逻辑则有算法内部实现。循环不用操心

算法处理的是容器的迭代器，这样的好处很明显，能够提高算法对容器的适应性。虽然容器各不相同，一个算法往往可以接受相容的多种迭代器

一个算法能接收的迭代器的类型时通过算法模板参数的名字来体现的。一个算法能接收与参数类型相容的所有种类的迭代器。
> 这里应该不是都是最大的权限即随机访问迭代器吧？这样虽然特别妙？不对指针不应该是指向基类吗？那么怎么的？感觉上应该是限定的

然后就规定一下显式的操作说明吧：
参数容器的作用范围应该一样。一般地，参数(src_first, src_last, dst_first)

自定义操作
有些算法要求使用者提供一个函数或函数对象作为自定义的操作条件，因为往往返回结果为false或true. 所以可以看作谓词。
Pred: 一元谓词，需要一个元素作为参数
BinPred:二元谓词，需要两个元素作为参数
eg: count_if(InIt first,InIt last, Pred cond);
> 注意这里的默认参数，感觉上应该与vector<int>x, 这里的int, 即class相关。
即一元谓词，二元谓词的参数。对的！

```
#include<numeric>
accumulate() // 累积算法,返回值
transform() // 变换，映射算法
```

发现越到后面越有点像数据库的内容了。

- 作业
STL算法应用——在学生容器中做统计
学生：name, sex, birth_place, major
功能：init, sort, display, match_major.
> match_major内容特别多，可以考虑用函数对象来做

```
利用函数对象来解决上面的问题
class MatchMajor
{	   Major major;
	public:
	   MatchMajor (Major m)
	   {	major = m;
	   }
	   bool operator ()(Student& st)
	   { return st.get_major() == major;
	   }
};
count_if(students.begin(),students.end(),
			              MatchMajor(COMPUTER))；
count_if(students.begin(),students.end(),
			              MatchMajor(PHYSICS))；
count_if(students.begin(),students.end(),
			              MatchMajor(XXX))；

// 扩展

    bool operator ()(Student& st)
    { return st.get_major() == major && st.get_sex() == sex;
    }

count_if(students.begin(),students.end(),
             MatchMajorAndSex(COMPUTER,FEMALE)) //计算机女生

// lambda表达式
count_if(students.begin(),students.end(),
        [](Student &st) { return (st.get_major() == COMPUTER)
			     && (st.get_sex() == FEMALE); })


//统计出生地为"南京籍计算机专业"的学生人数
cout << "出生地为\"南京\"的学生人数是：" 
      << count_if(students.begin(),students.end(),
  [](Student &st) { return (st.get_major() == COMPUTER) 
          && (st.get_birth_place().find("南京")!= string::npos);})

```
这里的思想很妙，但对象作为函数传递时，默认调用的就是A()，无参数的，所以此时就显示出了重载()运算符的好处

> 当实体完全是为了存储数据使用的时候，此时就演化为了数据库这门课程