---
title: python-study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:17:33
tags:
- python
- study
categories:
- [python, study]
---
## 1. vscode的好处

1. 显示思路过程
2. 实时性显示
3. 块编程
4. 批处理


## 2. 机器学习常用的库
- 核心库与统计：Numpy、Scipy、Pandas、StatsModels。

- 可视化：Matplotlib、Seaborn、Plotly、Bokeh、Pydot、Scikit-learn、XGBoost/LightGBM/CatBoost、Eli5。

- 深度学习：Tensorflow、PyTorch、Keras。

- 分布式深度学习：Dist-keras/elephas/spark-deep-learning。

自然语言处理：NLTK、SpaCy、Gensim。

数据抓取：Scrapy。

## 3. python使用总结

jupyter
- b? 查看文档
- b?? 查看源码
- %shell命令 
    - qucikref 快捷查看

### 3.1 基本类型

#### 3.1.1 null类型 
注意一个想法，python中万物均可对象化，这么说的意思就是所有东西都有它的类型和值。
> 特别需要注意的是，哪些时候使用引用即可，哪些使用必须使用一个新的对象

> dir() 查看参数的方法, type() 查看类型, id()=hashCode,
- ==: value equality
- is: reference equality
> is是==加上两个对象的位置是一样
> == isinstance(some_object, (int, float)) && value

None表示不是对象？什么时候会碰到？？
> 比如函数参数时，什么都没有传递过来，都是有默认值的情况！！！
> 表示空对象, 类型是NoneType, 值是为空
> if(None) =False // 一般是None的话，最好事先说出。
> True和Flase类型也跟None不一样，他们是bool类型

NULL: 表示空字符，str

"":
0:
False:
True: 

#### 3.1.2 值和引用
- 值类型: 数值、字符串、元组(注意这里是元组)
> 即只能自己改自己，不能因为赋值就改变自己，因为赋值它始终是产生了一个新的内存地址

- 引用类型： 列表、集合、字典
> 本身允许被改变

#### 3.1.3 and, or替代了&&, ||;  &,|依然是按位操作， 它的优先级始终是小于比较，大于赋值的
> python中int之间的and, or; 始终是按照先将其转化为bool类型，然后按照实际的and,or判断逻辑，不行则返回即返回值

#### 3.1.4 

- 可执行的伪代码
- 使用缩进而不是括号
- 万物皆可对象
- 动态引用，强类型
- 鸭子类型，不关注具体类型，而是关注能做什么
- 属性和方法
    - getattr(a, attr)(x): 反射，属于强类型
- 可变 vs 不可变
- 单值，即标量类型
    - None
    - str
    - bytes
    - float
    - bool
    - int
- 数值类型
    - **, 平方
    - 7.2324
    - 6.78e-5
    - 3 / 2: 实际的除法
    - 3 // 2: 整除
- 字符串
    - ', ", """
    - r' ',  "\\"  字面值和转义
    - 不可变的，即s[1]=2是错误的。
        - hash(): 可行；
        - =：产生一个新的对象
    - 基本操作
        - 长度 len()
        - 清空 str=""
        - 判断某种属性
            - isalnum() // 数字和单词
            - isalpha() // 单词
            - isdecimal() // 数字 byte
            - islower() // 小写
        - 字母相关操作
            - lower()
            - captialize()
            - upper()
        - 常见操作
            - 查找
                - 次数， count(substr,start, end)
                - 位置
                    - index(str, ) rindex  // 异常
                    - find(str, ) rfind // lower,  -1
                    - endswith, startswith
            - 去空格
                - strip(),  rstrip()
            - 划分为列表： split,  rsplit
            - 替换， replace(A, B)
            - 合并两个字符串
                - join
            - 格式化
                - {0:.2f}, {0:s}, {2:d}
            - encode, decode


### 3.2. 元组(), 集合{}, 列表[], 字典{}

#### 3.2.1 list []
- 长度
> 特别地，长度一般是有多种方法，具体哪种一般来说看源代码怎么定义的，对于Tree结构，习惯性地包括定义size,那么使用size方法绝对是最优秀的
    - __len__方法
- 插入
    - 尾部， apppend
    - 任意， insert(index, value) 
- 删除
    - 尾部， pop()
    - 任意， del li[index]
    - 值， remove(value)
- 查找
    - 单点查找
        - 是否存在 in
        - index:  li.index(value)
        - value:   li[index]
    - 范围查找
        - d[start:end:step] // 可为负
- 合并list
    - +性能低
    - extend(list2)
- 其他
    - li.sort(key=) // 原地排序
    - sorted(li) // 返回副本
    - reversed

> 允许范围查找，赋值，删除，即切片

#### 3.2.2 dict {}
> key必须是可以hashtable的数据，即唯一性, 可以通过hash来检验; 一般来说只有值类型可哈希
> __doc__ , 查看文档中的使用说明，输入与输出
> isinstance(1, set) 查看是否是需要的类型

- 插入
    - set(key, defalut) // 存在不改变值
    > 可加后续操作 set(key, default).appende(key)
    - di[key]=value  // 存在改变值
- 删除
    - del d[key]
    - di.pop(key) // 返回当前元素
- 查看
    - 存在： in di // 默认在keys()中查看
    - di[key] // KeyError 异常 
    - do.get(key, default_value) // 不存在返回特定的值
- 合并dict
    - update(di2)  // 完全舍弃旧值
- 输出
    - di.keys() // Iterable类型
    - di.values()
- 由两个列表创建
    - di = {k:v  for k,v in  zip(list1, list2)}  // 会吞并重复元素
    - di = dict(zip(list1, list2))
    > li = list(zip(list1, list2)) // [( ), ( )] 此时内部为元组
- 分解为两个列表
    - list1, list2 = dict.keys(), dict.values() // python3是对应的顺序
    > list1, list2 = zip(*li)  // li为列表时

- 一些进一步简化的操作
    - defaultdict // 可设置value默认值
    > from collections import defaultdict
    > by_letter=defaultdict(list) // 中间传入的为类型

> 一个很神奇的现象, di = {1:2, 2:5} ci=di
> 1. di = {} ,  ci => {1:2, 2:5}
> 2. di[1]=5  ci[1]=>5

<font color="red">defaultdict</font>
> 可以用来指定value的类型

#### 3.2.3 集合 set{}
> 特殊的字典，只有健没有值; 同样不可重复. 
> 没有特定的位置性.  像元组则是序列数据，即可重复出现

- 插入： add(x)
- 删除： remove(x)
- 清空： clear()
- 合并集合： a.update({1,2})
- 清楚首元素： a.pop() // 默认最小元素， 可以做堆
- 两个集合的运算
    - A是B的子集： A.issubset(B)
    - A是B的父集： A.issuperset(B)
    - 并集
        - |
        - union
    - 交集
        - &
        - intersection
    - 差集， -,  不够减为空
    - 异或 ^
    - 支持 &=

#### 3.2.4 推导式
- list 

- tuple: !!! 有个特别好的特性就是generator, 即满足惰性求值; 而且是值类型
> 所以这里得到的结果不是tuple类型，而是generator

- {} 结果两种
    - dict
    - set

#### 3.2.5  复制
- copy只复制一层
- 深度复制
> import copy
> di = copy.deepcopy(my_dict)

#### 3.2.6 一些好的方法
- __mro___ ：动态类型， 判断属于哪些类型
- isinstance(A, type): 判断是否属于某个类型
- if,  __len__, 查看是否为0,  and都是转变为bool类型来做
- __doc__: 查看基本的函数输入，输出文档
- dir(): 查看所拥有的方法

### 3.3 语句

- if A: pass  elif: pass else: pass
- while A: pass
- for i in range(2):
- try:   except Exception:  finally
### 3.4 函数

#### 3.4.1 基本
- 参数
    - 顺序性， 有默认值的放在最后
    - *args, **kwargs  默认为None
    - 可为函数
    - 字典式指定值
    > 无参数时，为None
- 变量： 严格作用域
- 返回值：
    - 多值
    - 字典

#### 3.4.2 匿名函数
`lambda paras: return_value`
`lambda x,y : x+y`
> 可以引用在任何需要使用函数作为参数的情况中
如 map,  reduce, sort
> reduce 还是不大好理解

#### 3.4.3 柯里化
部分参数应用
 ```
 add_number = lambda x,y: x+y
 from functools import partial
 add_five = partial(add_numbers, 5)  // 相比于默认参数，是动态默认参数
 ```

 #### 3.4.4 生成器
 generaotr 惰性求值
 - return => yeild
 - gen = ( )

 #### 3.4.5 itertools模块
 > 用于许多常见数据算法的生成器

https://www.liaoxuefeng.com/wiki/1016959663602400/1017783145987360

 - combinations(, k): 产生组合数
 - permutations(): 产生排列数
 - groupby, 将相同的字符串给弄出来
 ```
>>> for key, group in itertools.groupby('AAABBBCCAAA'):
...     print(key, list(group))
...
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']
 ```
 - chain(iter1, iter2[,..]): 串联多个iter
 - repeat('A',n): 重复n次
 - cycle("ABC"): 循环，直到ctrl+c退出 
#### 3.4.6 字节、位运算
(n & 0x3f3f3) >>1: 人可用

### 3.5 类
- 命名特点
    - 属性: 名词
    - 方法：动词, 注意要加括号
    - 

### 3.6 其他模块

#### 3.6.1 正则表达式
- 模式: r"  "
    - 替换：str = re.sub(origin_str, res_str, str)
    - 匹配首个结果 re.match(, test) None(False)
    - 划分 li = re.split(r' ',str)
    - 分组 li = re.group(r' ', str)  //group(0)为原始, 1-k为分组结果
- 方法
    - 单个字母匹配
        - \d 数字
        - \s 空格
        - \w 字母或数字
        - . 任意
        - 0~9,a~z 特别
        - \. 转义， 匹配原字符
    - 次数
        - a  1次
        - a+ >=1
        - a* >=0
        - a{n} n次
        - a{n,m} n~m次
    - 特别
        - ^\d: 指定必须从数字开头
        - \d$: 指定必须数字结尾
    - 范围
        - [ABC]: A,B,C任意模式
        - (A|B)： 两个可选
        > () 子表达式的开始和结束，在分组时起到特别的作用，一个括号表示一个group
- 特别说明：正则表达式匹配为贪婪模式

#### 3.6.2 文件模块
```
for i in open(path): // i默认有行结束符，记住用rstrip()
```
- f.read(121)
- f.tell()
- f.seek(3): 移动指针到指定字节

- 模式： r, w, a, r+, b, U

#### 3.6.3 collections
- namedtuple
> 为tuple的每一项起名字
```
from collections import namedtuple
Point = namedtuple('Point',['x','y'])
p = Point(1,2)
p.x  # 1
p.y # 2
```

- deque
> list支持的太过分, deque缩小限制，只允许两端插入和删除
    - append, appendleft
    - pop , popleft

- defaultdict
> 设置默认的值
dd = defaultdict(lambda: "N/A")

- OrderedDict
>  保证先进先出的顺序，可以当做队列用

- ChainMap
> 将多个dict, 合为一个逻辑dict
## 4. 优化

https://python-web-guide.readthedocs.io/zh/latest/idiom/idiom.html#true-false-none

### a. 链式：1<a<7

### b. b=a if a>b else b

### c. x,y=y,x

### d. str="myname:{} my age {}".format(name, age)

### e. list, dict使用[], {}

odd_list=[e for e in mulist if e%2==1]

user_list = [{'name': 'lucy', 'email': 'lucy@g.com'}, {'name': 'lily', 'email': 'lily@g.com'}]
user_email = {user['name']:user['email'] for user in user_list if 'email' in user}

### f. if None 特别说明
if() : pass 实际调用的是l.__len__()==0

if something is None:  // None是单例对象
// 注意

### g. index变量访问, enumerate;  
```
for index,  element int enumerate(my_container)
```
### h. 避免使用可变变量作为函数参数的默认初始化值, None
// 可以使用None作为可变对象的占位符

### i. 函数参数即函数
// 一切都可对象，可以把参数作为函数使用
```
import operator as op
def print_table(operator):
    for x in range(1, 3):
        for y in range(1, 3):
            print(str(operator(x, y)) + '\n')

for operator in (op.add, op.sub, op.mul, op.div):
    print_table(operator)

```
### j. 防御式编程, 不断检查参数

### k. dict实现switch..case..
```
# bad
def apply_operation(left_operand, right_operand, operator):
    if operator == '+':
        return left_operand + right_operand
    elif operator == '-':
        return left_operand - right_operand
    elif operator == '*':
        return left_operand * right_operand
    elif operator == '/':
        return left_operand / right_operand
# good
def apply_operation(left_operand, right_operand, operator):
    import operator as op
    operator_mapper = {'+': op.add, '-': op.sub, '*': op.mul, '/': op.truediv}
    return operator_mapper[operator](left_operand, right_operand)
```

### l. namedtuple ?
```
# bad
rows = [('lily', 20, 2000), ('lucy', 19, 2500)]
for row in rows:
    print '{}`age is {}, salary is {} '.format(row[0], row[1], row[2])

# good
from collections import  namedtuple
Employee = namedtuple('Employee', 'name, age, salary')
for row in rows:
    employee = Employee._make(row)
    print '{}`age is {}, salary is {} '.format(employee.name, employee.age, employee.salary)
```

### m. isinstance
### n. with
### o. yeild关键字, 惰性求值
([ expression ]) // 本身就是惰性求值
```
# bad
def f():
    # ...
    return biglist

# good
def f():
    # ...
    for i in biglist:
        yield i
```

### p. Wraper
```
from functools import wraps

ef beg(target_function):
    @wraps(target_function)
    def wrapper(*args, **kwargs):
        msg, say_please = target_function(*args, **kwargs)
        if say_please:
            return "{} {}".format(msg, "Please! I am poor :(")
        return msg

    return wrapper


@beg
def say(say_please=False):
    msg = "Can you buy me a beer?"
    return msg, say_please

```

## 5. 好用的工具

https://blog.csdn.net/moqsien/article/details/79544876


## 6. 一些其他

### 6.1 二分搜索和维护已排序的列表
bitsect: 二分搜索

bitsect.bitsect(c, 2) // 搜索插入的位置
bitsect.insort(c,5) // 插入元素

### 6.2 排序
a.sort(key=len)
sorted(a)

### 6.3 zip: 将多个列表、元组或其它序列成对组合为一个元组列表; zip(*list), 解压
seq1 = ['foo', 'bar', 'baz']
seq2 = ['a', 'b', 'c']
zip(seq1, seq2)
> type zip

a = list(zip(seq1,seq2))

解压
seq1, seq2 = zip(*a)

### 6.4 reversed翻转

另外，注意range也是一种类型

关于relu的倒数可以直接用

## 7. 关于Python解释器

1. 脚本语言 vs 编译语言

首先，脚本语言需要能够实时运行，与编译语言来说有何不同？ 脚本语言需要进行保存上下文变量


> 特别地， C vs Python
    1. 没有类型： 在实现阶段，那么需要进行多层的类型匹配。这会花费大量的时间
    2. 函数参数？ python中对参数是如何进行解释的？ 如果按照python来参数是进行怎么解释的
    3. ？

2. 解释器
？ 解释器是如何实现的
 - CPython: 使用C语言对python进行解释
 - IPython: 在CPython的基础上向上封装了一些高级接口
 > 另外，还有一些其他的接口，比如JPython, 用jav来做


 ## 8. 关于其他
 当显示Microsoft Visual C++ 14.0 is required.时，可到以下网站进行安装
 https://www.lfd.uci.edu/~gohlke/pythonlibs/#pygraphviz