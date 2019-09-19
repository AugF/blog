---
title: nju-course-cplus-lecture1-summary
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:45:52
tags:
- nju
- course
- cplus
categories:
- [course, nju, cplus]
---
> 静态数据区：全局变量、static存储类的局部变量以及常量的内存分配；
> extern int x; 引用其他文件的全局变量，注意初始值。
> 使用说明： 尽量使用本地变量static; 全局变量赋初始值；外部全局变量使用extern.

# 上节课重点回顾
1. 程序=算法+数据结构
2. 代码区和栈区(普通局部变量)、堆区(malloc使用的变量)
```
//file1.cpp
int a=1;

//file2.cpp
extern int a; //
int main(){
    a=100;
}

new同malloc, 分配的变量在堆区中
```
3. 再理解循环， 以及递归调用。实际执行（中序遍历）。循环,退出条件。多种情况，基本及其他。返回值，参数忌大值
```
while(){

}
for( ; ; )
共享全局变量

int recursion(int a) {
    if(a in basic) solve;
    else {
        recursion(a/2);
    }
}

```
4. 引用，名字。指针，链表
```
int f(int *b){
    int c=3;
    *b=c; // 在b未改变指向的地址之前做赋值。能够赋值成功
    b=&c; // 这样操作没什么用，如果返回b的值可能变了。
}

main(){
    int a=1;
    inf *b=a; // 这里初始化等价于int *b; b=&a;
    f(&b); // 1->3
}
// 指针没有那么恐怖，主要是看在哪定义的，即作用范围。如函数类以及参数，那么作用范围只不过是函数内而已。

int g(int &x){
    int m=1;
    x=&m // 错误
    x=m; //这里附带着就把原值给改变了
}
main(){
    int a;
    g(a);
}

对于数组，同指针一样。指针就理解为链表，当选进去之后，出来还是首地址，但是该改变的都改变了
void fun(int a[]){
    a[1]=1;
}
int main() {
    int a[]={1,2};
    fun(a);
    cout<<a[1]; // 1
    return 0;
}
```
5. 序列数据的表示，数组，链表，更多
```
数组 int a[N];
单位长度 a+i*sizeof(int)

```
6. 链表
    - 重新分配
    - 访问元素，顺序，值
    - 增加元素
    - 删除元素
```
file1.h
struct Node{
    int content;
    Node *next;
};

不带头结点的单链表
这里首先设计的就不合理。

Node* find(Node *head,int i){ 
    //？这里参数和返回值的类型
    if(!i) return head;
    else{
        //访问
        Node *p=head;
        int j=0;
        while(p!=NULL & j<i){
            p=p->next;
            j++;
        }
        if(p!=NULL) return p;
        else return NULL; // 表示未找到
        // return p;
    }
}

Node* findByValue(Node* head,int value){
    if(head.content==value) return head;
    else{
        Node *p=head;
        while(p!=NULL&p->content!=value)
            p=p->next;
        return p;
    }
}

bool insert(Node *head, int value, int i) { // 按所给的位置i插入value，不用返回值。
    Node *q, *p; // 数组才需要new, 单个变量应该不需要。而且新定义的结构体也应该没有被定义在系统内吧
    *p=head;
    q->content=value;
    if(i=0) { // 注意这里是没有头结点的情况
        q->next=p;
        head=q;
    } else {
        while(p!=NULL & i-1){ // 从0开始, 注意是找前一个
            p=p->next;
            i--;
        }
        if(p==NULL) {
            delete q;
            q=nullptr;
            // q使用结束后，将其赋值为空。防止多次delete,还有后续使用
            return false;
        }
        else{
            q->next=p->next;
            p->next=q;
        }
    }
    delete p,q;
    p=nullptr;
    q=pullptr;
}

void deleteP(Node *head, int value, int i) {
    Node *p;
    if(i==0){
        p=head->next;
        head=p;
    } else {
        while(p!=NULL & i-1){ // 从0开始, 注意是找前一个
            p=p->next;
            i--;
        }
        Node *q=p->next; // 如果是最后一个也没关系，不过指向空而已
        p->next=q->next;
        delete p,q;
    }
}

// Node q; q.content
// Node *q; q->content 
// 猜想是对的，箭头左边是指针；点号左边是实体
```
7. 命令
    - include
    ```
    路径搜索策略:
    1. 当前目录
    2. 参数-L指定的目录
    3. gcc的环境变量CPLUS_INCLUDE_PATH
    4. gcc的内定目录
    gcc -E -v --prefix=..
    prefix/include
    prefix/local/include
    prefix/lib/gcc/--host/-version/include

    #include<a.h>
    从第2步开始
    #include "a.h"
    从第1步开始
    ```
    - define
    ```
    #define MAX(a,b) ((a)>(b))?(a):(b)
    #define M 100

    ```
    - ifdef,  if
    ```
    file1.h
    #define ABC
    int a;
    void fun();

    file1.cpp
    #include "file1.h"
    void fun(){
        a=1;
        cout<<a<<endl;
    }

    file2.h
    #ifdef ABC
        int b;
    #else
        int a;
        fun();
    #endif
        int c;

    file2.cpp

    ```
