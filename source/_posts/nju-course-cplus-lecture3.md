---
title: nju-course-cplus-lecture3
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 09:49:11
tags:
- nju
- course
- cplus
categories:
- [course, nju, cplus]
---

# 本节课重点

## 操作符重载

比如对于复数来说，怎么实现加法？
- 成员函数add
- 全局函数

不符合数学上的习惯

不可以重载的符号：“. ”， “.* ”，“?: ”，“:: ”，“sizeof ”

原则：不改变操作数个数，不改变原操作符的优先级和结合性。

双目操作符重载：
<返回值类型> operator # (<类型>); //#代表可重载的操作符
friend Complex operator + (const Complex& c1, 
							    const Complex& c2);


单目操作符重载：
<返回值类型> operator # (); 
friend bool operator !(const Complex &c);

Counter& operator ++()  //前置的++重载函数
		{	value++;
			return *this;
		}

	const Counter operator ++(int)  //后置的++重载函数
		{	Counter temp=*this; //保存原来的对象
			value++; //写成：++(*this);更好！调用前置的++重载函数
			return temp; //返回原来的对象
		}

- =重载
隐私的赋值操作符重载函数：组个成员进行赋值操作
- 对于普通成员进行常规的赋值操作。
- 对于成员对象，则调用该成员对象类的赋值操作符重载函数进行赋值操作
- 对于基类成员，则调用基类的赋值操作符函数进行赋值操作。

特别注意地是，等号的情况是特别容易出错的情况。因为这里对指针的操作就是
```
class A{
	int a;
	int b;
	int *p;
public:
	A(){}; 构造函数
	A(const A&a); //拷贝构造函数
	~A();
}

A m,n;
m=n;  // 实际上的操作就是 m.a=n.a, m.b=n.b, m.p=n.p. 最后一个就有问题了 
```
> 记住的就是拷贝构造函数和构造函数就是一样的，这里，不过参数不一样。拷贝构造函数参数是本身。 
> 赋值运算和

A& operator = (const A& a)
{	if (&a == this) return *this;  //防止自身赋值。
	delete []p;
	p = new char[strlen(a.p)+1];  //注意这里指针的赋值，首先删除原有的，然后，根据长度重新找，再赋值
	strcpy(p,a.p);
   x = a.x; y = a.y;
	return *this;
}

- []重载
int &operator[](int i) //访问向量第i个元素。
		{	return p_data[i];
		}

略去new, delete, ()， ->的重载

- 作业： 独立完成字符串的一系列动作。

## lambda表达式

求定积分的函数：试图把函数作为参数。
C++11
double integrate(double (*f)(double),double a,double b);

integrate([](double x)->double { return x*x; },0,1);

- 常用格式
[<环境变量使用说明>]<形式参数><返回值类型指定><函数体>

<环境变量使用说明>：指出函数体中对外层作用域中的自动变量的使用限制：
空：不能使用外层作用域中的自动变量。
&：按引用方式使用外层作用域中的自动变量（可以改变这些变量的值）。
=：按值方式使用使用外层作用域中的自动变量（不能改变这些变量的值）。
&和=可以用来统一指定对外层作用域中自动变量的使用方式，也可以用来单独指定可使用的外层自动变量（变量名前可以加&，默认为=）。

> 理解：将lambda表达式看作是函数对象来实现
> 怎么用啊？需要找个例子
- 作用于实参
cout << [](int x)->int { return x*x; }(3);
- 传给其它函数
f([](int x)->int { return x*x; }); // 这里依然不是很明朗，需要式子

## 继承-派生类

对事物按层次来分类，对概念进行组合，支持软件的增量开发。

继承的目的进行软件复用。
如何扩展？
1、修改源代码。
2、继承。对一个面向对象的程序，在定义一个新的类时，把已有程序的一个或多个类的功能全部包含进来，然后再给出新功能的定义，或者重新定义。目标代码复用
派生类拥有除构造函数和赋值操作符重载函数外的所有成员
友元不能通过集成传递给派生类。
私有成员不能传递。
public-外界，即对象，实例， protected-面向继承
> 继承和封装的矛盾

> protected的使用，用于今后不太可能发生变动的、有可能被派生类使用的、不适合对实例用户公开的成员声明

> 有个问题，既然private不能直接访问，那么复制过来又有什么用？按说明是需要复制过来
而且还占空间

> 如何是同名函数，则访问基类同名成员时要用基类名受限A::f()。但也可使用using A::f;声明后直接使用

> 派生类的作用域大于基类的作用域

> protected, 内部和子类，以及子类。而且是兼容的状态

> 父对象、子对象，对于包含的状态，可以允许

> 派生类对象，先初始化子对象
> 拷贝构造函数和赋值都是采用默认的形式

实例：一个公司中的职员和部门经理类的设计

- 分析
职员  工号、名字
```

```

## 多态性

具有public继承关系的两个类，在C++中存在下面的多态：
- 派生类对象的类型既可以是派生类，也可以是基类
- 基类的指针可以指向派生类或基类
> 其实就是一个意思。
- 一个可以发送到基类对象的消息，也可以发送到派生类对象，怎么做到？缺例子？


由上面的知识可以带来消息的绑定问题。
> 向基类的指针或引用所指向或引用的对象发送消息。

> 问题：只要显式在参数中写定了基类还是父类，发现实际。

- 虚函数
virtual void f(){
}
1. 实现消息的动态绑定
2. 指出基类中可以被派生类重定义的成员函数

构造函数不能是虚函数，析构函数可以是虚函数。只要在基类中说明了虚函数，在派生类、派生类的派生类，同型购的成员函数都是虚函数
> 意思是基类中将某个函数设置为虚函数，那么不管它继承到哪个位置，它将会一直是虚函数。 

只有通过基类的指针或引用访问基类的虚函数时才进行动态绑定。

> 当基类设置为纯虚函数后，之后的凡是基类指针，因为自身的函数设置为了虚函数。所以，它直接就指向了派生类的对象。

何时需要定义虚函数？
> 基类设计的不好，希望使用派生类。
> 在基类中根本无法给出实现，子类往往会对应于不同的情况
（纯虚函数）

- 纯虚函数
virtual int f()=0;
包含纯虚函数的类叫做抽象类，不能用于创建对象。
从某种意义上感觉像是接口的感觉。

一般地，会把所有的函数都设置为纯虚函数，即
```
class Figure {
	public: 
		virtual void draw() const=0;  // 这里const是做何用的， 是注重返回值是定值吗？
		> 等下好像前面加了const, 是就限制了不能改变元素。所以，外部的全局函数就可以直接使用
		virtual void input_data()=0;
}
```

一般的应用
```
- 图形的输入
for (count=0; count<MAX_NUM_OF_FIGURES;	count++)
{	int shape;
	do
	{	cout << "请输入图形的种类(0：线段，1：矩形，2：圆，-1：结束)：";
		cin >> shape;
	} while (shape < -1 || shape > 2);
	if (shape == -1) break;
	switch (shape)
	{	case 0: //线
			figures[count] = new Line;	break;
		case 1: //矩形
			figures[count] = new Rectangle; break;
		case 2: //圆
			figures[count] = new Circle; break;
 	}
	figures[count]->input_data(); //动态绑定到相应类的input_data
}

- 图形的输出
for (int i=0; i<count; i++)	
	figures[i]->draw();  

```

此时可以看出，代码完全不需要改动。只需要将类的实现进行改变而已，很好地提高了效率。

考虑一种低效的实现方式，即使用union来做，可以发现，当增加新的图形种类是，需要修改下述代码，增加分支
```
图形数据的输入：
	int count;
	for (count=0; count<MAX_NUM_OF_FIGURES; count++)
	{	int shape;
		do
		{	cout << "请输入图形的种类(0:线段,1:矩形,2:圆,-1:结束):";
			cin >> shape;
		} while (shape < -1 || shape > 2);
		if (shape == -1) break;
		figures[count] = new TaggedFigure; //空间利用效率不高！
		switch (shape)
		{ 	case 0: //线
				figures[count]->shape = 0;
				input_data(figures[count]->figure.line);
  	 			break;
			case 1: //矩形
				figures[count]->shape = 1;
				input_data(figures[count]->figure.rect);
 				break;
 		  case 2: //圆形
				figures[count]->shape = 2;
				input_data(figures[count]->figure.circle);
  	 		break;
	 	} //end of switch
	} //end of for


图形的输出：
	for (int i=0; i<count; i++)
	{	switch (figures[i]->shape)
			{ case 0:
						draw(figures[i]->figure.line);
						break;
				case 1:
 						draw(figures[i]->figure.rect);
						break;
				case 2:
 						draw(figures[i]->figure.circle);
 						break;
 			}
	}

```

- 作业：用抽象类为栈的两个不同实现提供一个公共接口

- 使用抽象类给类提供一个抽象接口
> 感觉上真的，抽象类实现真正意义上的抽象。

class I_A{
	public:
		virtual void f(int)=0;
};

void func(I_A *p) {
	p->f(2); // 这里不知道p所指向的对象有哪些数据成员，因此无法访问它的数据成员。而且保证了类的封闭性。
}

> 从某种意义上感觉对事件机制有了进一步的理解。
所有都由一个父类继承而来。而虚函数所定义的就是信号，就可以看作基类指针，完全不知道到底派生类的谁会接收它。

代码复用的另一种方式——聚集
继承实际就是把一个类的代码复制到另一个类中来实现，具有继承关系的两个类之间通常属于一般与特殊的关系。
继承不是代码复用的唯一方式，有些代码复用不宜用继承来实现。如：“飞机”类复用“发动机”类。

具有聚集关系的两个类之间通常属于整体与部分的关系。

> 访问控制仿佛如同一种过滤器一样；只能过滤到和当前一样的结果。
private只对自己使用。
protected针对继承元素。

> 之前就一直在想一个问题，可能让你一直继承下去吗？我这个世界可不想你做这些东西？
> 实际社会上还会存在哪些要求呢？

> 实际上来说的话，那么当一个类使用了private继承就是想不要再被其他继承下去。继承下去有毛线用，你又访问不到，所以说啊，你还不如好好使用的，有什么意思，对吧！
顶多是你可以.后再加上A::的作用域进行访问。如f()是A的private成员，对于B继承A，访问f()即为`b.A::f()`

继承与封装存在矛盾，聚集则否。
继承提供两种接口，而对于聚集，一个类对外只需提供一个接口

### 多继承

如何实现？
- 采用单继承，概念混乱，导致A和B之间增加了层次关系。易造成不一致。
- 聚集，不能实现子类型

顺序
class C: public A, public B{}
即按照继承的声明方式。

对于基类的指针会自动进行地址调整，也就是自动指向对应的存储空间。
函数名混淆，采用基类名限制。

重复继承问题，原本设置不对，应该把重复继承的部分设置为基类。

一种新的方式：
class A{
	int x;
public:
	A(int i){x=i;}
}

class B:virtual public A{  // 注意没？这里出现了虚拟继承的概念
	int y;
}

间接包含虚基类的类：

虚基类的构造函数由该类的构造函数直接调用。（即虚基类的构造函数由最新派生出的类的构造函数调用）
虚基类的构造函数优先非基类的构造函数执行。

虚基类的实现其实比较复杂，并不是没有它了。而是将x移到最后，在原本x的位置存储了一个x的偏移量指针。

非纯虚基类，就是有自己的实现。但是允许你去用派生类的。因为自己做不好的原因。
### todo

- 从上面的过程话说设计模式

> 那么这个世界的组织形式是什么？
分离，谁是谁的什么，还有或者谁组成了谁