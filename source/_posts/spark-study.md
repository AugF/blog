---
title: spark study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:18:49
tags:
- spark
- scala
categories:
- [spark, study]
---
sc.textFile(,numpartition)

如果numpartition参数比实际划分的SplitSize小的化，则按照SplitSize的数目在每个executor上执行；否则就按照numpartition参数进行执行；

一定要区分资源调度和任务执行

在任务执行阶段，每个executor只会按照其对应的InputSize单位来执行数据。如果有多个executor, 则按照多个的executor来进行执行

# 写代码要考虑的事
代码之前——环境，环境如何配置？
代码载体——IDE, 有哪些特点？
代码语言——常用操作和模板进行总结，语法特性要能表达出来，用语言能够实现出任意自己想实现的犯方法
  - 角度1
    - 数据结构
    > 1. 对基本数据结构操作的了解
    > 2. 这些操作的输入、输出参数是什么？如何实现的？快慢如何
    > 3. 如何自己定义？
    - 算法
    > 设计算法，算法正确性，算法的时间空间复杂度，算法的实现，代码复杂度
  - 角度2
  基本数据结构，字符串，数组，元祖，列表，字典这些抽象的数据结构；
  对象，多态，操作符重载，
  异常，
  debug


代码调试——如何测试自己的代码是否正确
    > 测试语句，继承测试
代码维护——Git, 版本控制
代码执行——编译器执行的角度（一般不用去考虑）
代码上线——命令行设置


> 非设计算法，主要调用接口，那么关注代码的含义，代码对应着实际上有哪些操作
```代码部分
import spark
// 前面一系列的环境配置先耍开

def main(){
    
    val conf = new SparkConf().setAppName().setMaster
    conf.set()  // 这里可以配置哪些项？这些项又表示着什么？

    val sc = new SparkContext()   // 2.3之后spark是怎么执行的

}
```


计算模式中节点故障和慢节点容错处理
特别是在流和交互式SQL查询中

RDD能够保存世系图，而且提供保存到各个部分的功能。
所以RDD能够很好地处理并行计算中的数据共享

spark编程中开发者需要编写一个驱动程序来链接到工作进程，驱动程序定义一个或者多个RDD以及相关行动操作，驱动程序同时记录RDD的继承关系。
工作进程是一直运行的进程

操作
- 定义操作，创建RDD，来自于内存集合和外部存储系统，转换操作生成的RDD
- 转换操作，只是转换，并未生成
- 控制操作，进行持久化
- 行动操作，触发spark运行的程序

分区，注意到分区是一个逻辑概念

分区的多少，往往意味着并行计算的粒度
如果是从本地文件创建，则默认值为程序所分配到的CPU数；如果是从hdfs上建立，则为文件的数据块数

RDD首选位置
Spark形成任务有向无环图时，会尽可能地把计算分配到靠近数据的位置，减少数据网络传输

## 2. scala
https://learnxinyminutes.com/docs/scala/

- :help 查看所有的帮助
- :type (true, 2.0)
  > 查看类型
- 保存和加载 repl文件
  - :save /sites/repl-test.scala
  - :load /sites/rep1-test.scala
- 查看历史 :h?
  > 怎么使用历史中的文件呢？
  - :history number
- 编写长代码
  - :paste 
  > 怎么加载文件中代码还是不会 
- 退出
  - quit

### 2.1 类型
- val z:Type = value
  > val, var
  - Type: Int, Long, Double
  - 类型继承体系
    ![](asserts/scala_type.png)
    - 列表中没有元素了即, Nil = List[Nothing], 它是List[T]的子类
    - Null类型的唯一实例是null值，可以将null赋值给任何引用但是不能赋值给值类型的变量
    - 与Java基本类型相对应的类，以及Unit类型，都扩展自AnyVal
    - Any类定义了，A.isInstanceOf[B] 来判断A是否是类别B
    - A.getClass.getSimpleName

- String
  > raw" " 不含转义字符
  - 基本
    - 属性
      - length
    - init
      - 直接赋值
      - Char[],   copyValueOf(char[], offset, count)
    - 增
      - 尾部增加元素或元组 str = str.concat(str2);  str = str+str2
    - 删
      - 转变为替换或者查拼接
    - 改
      - 改特定群组的值  str.replace(regex, "") //前面可以接正则表达式
    - 查
      - 按索引查 str(i)  str.charAt(i)
      - 按值查  str.indexof(c1)  str.lastindexof(c1)
      - 查范围的值 
        - 只要求前面  str.take()
        - 只要求后面 str.drop()
        - 同时  str.substring(start_index, end_index)  str.slice()
      - 查是否满足要求
        - str.startsWith()   str.endsWith()
  - 高级
    - 预处理  trim()
    - 切分 split()
    - 格式化  
      - s"${expression}" expression可以为上文的变量之类的
      > 对于输出的各种类型的操作为使用变量的toString参数
      - f"${}%1.0f dada"
      - raw"  "
      - regr.r内置了正则表达式, 如 `"(.*)@(.*)".r`

- 函数 def fun(x:Int, y:Int=4):Int={}
  - 默认参数
  - 可变参数, 由变量直接指定
  - 匿名函数 val sq: Int->Int   =  (x:Int)->x*x
  - 允许多返回值
  - 可以省略参数，可以省略返回值

- flow control
  - range(5): (1 to 5 by 1) start to end by steo
  - 循环
    - ().foreach{}
    - while() {}
    - for(a<-array){}
  - 条件
    - 简单本 val a = if(x==10) "" else " "

- Data Structures
  - 物理: 数组、链表
  - 不变的
    - Array()  List()
      - sortBy,  sortWith{case (a,b)} // 自定义
      - distince
    - Map()
      - sort: Map()没有sort函数，所以需要
      用list
      - 获取值
        - apply() 返回默认方法，不知道谁制定的,  default() 会进行定义默认值
        - getOrElse(v,defalut) 返回默认值
      - 增加，以及更新
        - 错误： map(key) = new_value
        > 注意这条命令只能对于mutable的集合可用
      - 删除
        - remove(key)
      -  其他
        - keys, values
        - isEmpty
        - max, min, filter, find
        - sum,size
        - ++添加一个新的
        - clear(), clone()
        - count(): 技术
    - Set()
      - 操作

- 类 Classes
  - 构造函数： 直接在类的开头声明
    class Dog(br: String) {
      var breed: String = br  // 这里即构造函数
    }
  - 变量和函数默认的访问控制权限是public
    > 特别地，可以private def fun(): Type={}  ？？？protect

- 同名Object, 实际上是该类的一个单例对象，在其他语言中往往对应的是静态方法，静态变量之类的东西。
  > 两者的区别感觉上说也就是Class需要new产生，而Object因为是静态的，所以可以直接使用

- case classes
  > vs Classes, 如何使用？ Classes强调的是封装，多态和行为。这些值在类中通常是私有的，方法是可扩展的。 // 即默认情况下
  > 而case classes的目的是来持有不可变对象，它们往往有很少的方法，而且这些方法基本上没有副作用。  使用起来定义简单，有点像结构体的感觉，pair
  case class Person(name: String, phoneNumber:String)

- trait 特征，实际上就是抽象类或者接口的概念；
  > 只需要定义值或方法的属性或者返回值即可。 由继承它的类来自行扩展
  class A extends Dog with Bark {}  // 扩展两个接口

- 继承
  > 继承利用的也是extends关键字，不过在scala中继承实体类有以下的要求：
    - def只能重写另一个def
    - val只能重写另一个val或不带参数的def(?,应该不会涉及到)
    - var只能重写另一个抽象的var
    > 可以定义自己的方法

- Pattern Matching
  > scala特有的, 利用了case结构; 这样的想法就是使得省去了break语句
```
 // Pattern matching might look familiar to the switch statements in the C family
// of languages, but this is much more powerful. In Scala, you can match much
// more:
def matchEverything(obj: Any): String = obj match {
  // You can match values:
  case "Hello world" => "Got the string Hello world"

  // You can match by type:
  case x: Double => "Got a Double: " + x

  // You can specify conditions:
  case x: Int if x > 10000 => "Got a pretty big number!"

  // You can match case classes as before:
  case Person(name, number) => s"Got contact info for $name!"

  // You can match regular expressions:
  case email(name, domain) => s"Got email address $name@$domain"

  // You can match tuples:
  case (a: Int, b: Double, c: String) => s"Got a tuple: $a, $b, $c"

  // You can match data structures:
  case List(1, b, c) => s"Got a list with three elements and starts with 1: 1, $b, $c"

  // You can nest patterns:
  case List(List((1, 2, "YAY"))) => "Got a list of list of tuple"

  // Match any case (default) if all previous haven't matched
  case _ => "Got unknown object"
}
```

- 函数式编程
  - map
  - foreach
  - filter  (1 to 15 by 2).filter(_%2==0)
  - reduce
  > 等价于 for{n<-s} yield sq(n)
  > // 所有scala这门语言本身就支持 惰性求值, 想想真的nb

- implicit
  > commonplace 频繁
  > 怎么说呢？ 实际上就是一种潜在的标记的感觉。默认的全局的变量。
  > 对于方法，如果有一个参数通常是平凡的，但是又可能改变。这是就可以使用implicit作为参数。
  > 编译器会自动向前寻找该标记然后进行解析
    - def sendGreetings(toWhom:String)(implicit howMany:Int) = toWhom + howMany
      > sendGreetings("wang")  // "wang 100"
    - implicit val myImplicitInt=100
      > ? 怎么用，不知道
    - implicit def myImplicitFucion(breed:String) = new Dog("Golder"+breed)
      > "wyp" // "Golder wyp"
  > 从理解上看，为什么不用默认形参或者指定参数，不会就只为了少点代码吧？？

- Misc
  - import scala.collection
  - import scala.collectin.immutable._
  - import scala.collection.immutable.{List,Map}
  - import scala.collection.immutable.{List=>Li}  // rename
  - import scala.collection.immutable.{Map=>_, Set=>_, _}  // 排序Map,Set

- Input / Output
  - 读 
    import scala.io.Source
    for(line <- Source.fromFile("myfile").getLines()) println(line)
  - 写
    val writer = new PrinterWriter("myfile.txt")
    writer.write("  " + util.Properties.lineSeparator)
    writer.close(