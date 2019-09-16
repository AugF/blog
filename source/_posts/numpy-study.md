---
title: numpy study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-12 19:41:55
tags:
- machinelearning
- numpy
categories:
- [machinelearning, numpy]
---

## 2. numpy

https://www.numpy.org.cn/

> python 太慢！！有个点就是关于python的切片都是复制，不是引用！不同numpy
> numpy基于C直接操作连续内存空间，比纯Python快10到100倍，并且使用内存更少

numpy提出的ndarry很好用,主要理解通用的数值数据处理上的强大功能，特别是在数组操作上
pandas主要用于统计和分析

！！！ 如何用整个数组的思想
> %time 可以直接统计时间

### 2.1 ndarry

- 属性与方法
    > 可以认为如果改变了属性，必然会返回一个新的结构
    - ar.shape: () 元组
        - di.reshape(): 改变形状, 返回一个新的数组
        > di.reshape() == np.reshape(di, 4)
    - ar.dtype: 
        - 基本类型
            - int64: 指的是位数, 64位，即8字节;  代码i8
            - float64  default 
            - uint8
            - complex64: 用两个32位的浮点数表示
            > ? 复数如何定义
            - bool:  代码?
            - object: python对象 代码O
            - string_: 固定长度  S10, 10字节，每个字符一字节
            - unicode_: U10
        - ar.astype(np.float64): 转换类型, 总会创建一个新的数组
    - ar.ndim： 有几维
    - ar.T: 返回转置，仍然未原数组的引用
    - ar.tranpose((1,0,2)): 轴对换，按照给定的元组进行重新轴的安排，比如获得新的数组
    - 转置=轴变换, ar.swapaxes(1,2): x,y,z; 这里就是对y轴和z轴进行变换

- 创建
    - 已有
        - np.array(sequence， dtype="")
        - np.asarray(arr) // 不确定是否是该类型
    - 特定矩阵
        - np.ones(shape) , ones_like(x): 根据shape和类型创建
        - np.zeros(shape) zeros_like  np.zeros((2,3))
        - np.eye(3) 单位矩阵
        - np.arange(10): 返回一维矩阵
        - np.linspace(a, b, nums): 返回[a,b]闭区间的nums个数的数据
    - 随机初始化
        - 随机数： np.random.randint(a, size)
        - 不同: np.random.choice(a, size=(4,))
        - np.random.shuffle(arr) 打乱现有的

- 运算: vectorization, 作用到元素级别
    - 基本操作
        > *, -, /, **, >(bool)， ==
        > 保证shape相同，不同用到广播
        - 基本操作上可以大做文章，比如跟已知的数组进行比较，
        - 条件之间可以进行 ==, !=,  ~,  &, | 等多种操作
        > 对于*, 如果两个操作树维度不等，按低维开始进行运算  *相等于 np.multiply
        ```
        data[name == 'Bob', 2:], 如果name是一维矩阵，矩阵的长度需要与比较的轴的长度相等
        mask = (names = 'Bob') | (names == 'Will'), 注意这里and,or无效，要使用&，|
        ```
        > 因此，可以通过布尔将数组中所有的值给置为某种操作。
    - 切片操作
    > 注意是原始数组的试图，即不会被赋值，任何其他操作都会反映到原变化. 即是等号也会如此
    > ? 如何复制,  reshape是否会改变原本的值
    > 看了下，是不能step的
    - 花式索引
        - arr[i]: 即如果是二维的，那么默认操作就会直接将该轴对应的值全都进行改变
        - arr[[1,2,3],[1,2,3]]: 可以直接返回对应的列和行， 直接进行切片
    - 扩展，赋值运算 拷贝arr.copy()
        - np.tile(arr, shape)
            > 如果shape的维数大于arr的维数，那么直接按shape的坐标来分配
            > 否则, 就考虑操作在对应维度上，比如先操作在行上，接着在列上，等等
        - np.repeat(3, 4) [3,3,3,3]
            > np.repeat(arr, [1,2], axis=0) // axis轴上, 第0行复制1次，第1行复制2次
    - stack 将arr进行合并，打包在一起
        - vstack((arr1, arr2)): 默认 axis=1, 组合在一起  = np.stack((arr1,arr2), axis=0)
        - hstack((arr1, arr2)): [1,2,3] , [2,3,4] => [1,2,3,2,3,4]: 一维扁平化的感觉
        - stack((arr1, arr2)): 高级，重新组合为性的数组

- 高级操作
    - 通用函数ufunc: 快速的元素级数组函数
        - 一元
            > 如果设置一个参数，返回array; 设置两个参数，在原数组操作; 当然二元除外
            - 幂函数 np.sqrt(arr) `arr**0.5`;  np.square(arr) `arr**2`
            - 指数函数, 底数函数 np.exp(arr)  np.log(arr) np.log10() np.log2(),  np.log1p(arr) `log(1+x)`
            > 这里参数可以是一个，即对一个数组进行统计；如果是两个矩阵，则对应元素级别做
            - 拆分小数 remainer, whole_part = np.modf(arr): 返回小数的小数，整数部分
            - 绝对值 np.abs, fabs:  对于非复数值，fabs更快
            - 获取符号 np.sign(arr): 获取符号
            - 对数进行近似整数化 np.ceil() 上;  np.floor() 下; np.rint() 四舍五入;
            - 判断值的类型，np.isnan() 不是一个数字的;  np.isfinite()   np.isinf
            - 三角函数 cos, cosh, sin
            - 逻辑非  np.logical_not  -arr
        - 二元 
            - 最值np.maximum(x,y) np.NaN为最终结果 np.fmax() np.NaN不考虑; np.minimum(), np.fmin()
            - 加减 np.add(), np.subtract()
            - 乘 np.multipy,  除np.divide, 丢弃余数 np.floor_divide, 向下整除; np.mod求余
            - 乘方 np.power
            - 传递正负号 np.copysign
            - greater,  greater_equal, less, less_equal, not_equal
            - &, | , ^ , ~可对数组使用，list不行, 使用np.logical_and, np.logical_or, np.logical_xor
    - 利用数组进行数据处理
        - 范围值  np.arange(start, end, step)
        - 网格数据 x,y = np.meshgrid(arr_1d, arr_1d)
        > 得到 arr_1d作为x轴的网格数据, 即x的每列元素相同
    - 将条件逻辑表述为数组运算 np.where(cond, true_ope, false_ope)
        > 将条件表达式的思想进行转变 `result = [(x if c else y) for x,y,c in zip(xarr, yarr, cond)]`
        - 数组比较res = np.where(cond, xarr, yarr) //后面两个不必为数组
        > cond为真, 按xarr操作；否则，按yarr操作
        - 值比较 res = np.where(cond, 2, -2)
        - 值、数组比较  res = np.where(arr>0, 2, arr)
    - 数学和统计方法
        > aggregation, reduction, 属于arr的操作; axis是站在留下来的数据角度思考的
        - 和，平均值 arr.mean(), arr.sum()
        > axis=0, 表示留下第一维,其余维度都会进行计算; 如果，如果没有参数，则对所有元素操作
        - 最值  max, min
        - 最值索引 agrmin, argmax, 同上
        - 标准差和方差 std, var
        - 累加和 累加积 arr.cursum() arr.cumprod , 同上, axis=0, 0维不进行操作
    - 用于布尔数组的方法
        - (arr==1).sum()  bool当做0和1
        - 存在 (arr==1).any()
        - 任意 (arr==1).all()
    - 排序
        - 就地排序 arr.sort() ; 返回副本 np.sort()
        > 参数为1, 对第1维进行排序
    - 唯一化及集合逻辑
        - 返回唯一元素 np.unique(x)
        - 返回公共元素 np.intersect(x,y)
        - 返回并集 np.union1d(x,y)
        - 返回是否包含的布尔数组 np.in1d()
        - 集合的差  np.setdiff1d()
        - 集合的对称差 np.setxor1d()
    - 用于数组的文件输入输出
        - np.save,  np.load
    - 线性代数 `numpy.linalg`
        - 矩阵相乘 x.dot(y)  np.dot(x,y)  x @ np.ones(3)
        - 对角矩阵的操作 arrdd=np.diag(arr1d)  arr1d=np.diag(arrdd)
        - 计算对角线元素的和 a.trace()  np.trace()
        - 行列式 det()
        - 本特征值、本征向量？ $D = U^T A U$  U^T, np.diag(A) = LA.eig(D)
        > 为什么有本?
        - QR分解？  qr
        - SVD分解？  svd
        - 矩阵的逆 inv
        - 解线性方程组  Ax=b,  x=LA.solve(A,b)
        - 最小二乘解？ x=LA.lstsq(A,b)
    - 伪随机数生成 import np.random as nrd 
        > np.random.seed(1234): 指定了全局状态; 那么每次设置这个值时，第一次都会产生相同的数,这个特点可以进行利用;
        > 想真正的实现随机数，1. 随机种子随机指定： 2. 使用np.random.RandomState, 创建一个与其他隔离的随机数生成器
        - 指定随机种子
            - nrd.seed(int) 随机种子
        - 返回某种数
            - nrd.permutation(arr/list1) 返回一个序列的随机排列或返回一个随机排列的范围
            - shuffle(arr/list1) 对一个序列就地随机排列
            - randint(a, size=shape) [0,a) randint(a,b, size=shape) [a,b)
            > 注意和python自带的randint不同[a,b]
        - 根据分布产生样本值，paras = (d1,d2,d3)
            - rand(d1,d2) 产生均匀分布的样本值 [0,1) 随机数的分布范围属于【0,1)
            - binomial 产生二项分布的样本值
            - normal(loc=0.0, scale=1.0) randn 产生正态高斯分布的样本值
            - beta 产生beta分布的样本值
            - chisquare 卡方分布
            - gamma Gamma分布
            - uniform [0,1) 均匀分布
        - 均匀分布  ranf = random = sample = random_sample [0,1)

    - 随机漫步
        > 一个简单随机漫步的例子：从0开始，步长1和-1出现的概率相等; 即取0,1之间的随机数,然后记录每一步的位置，就可以认为走路的过程为随机漫步
        > 多维，一次模拟多次随机漫步; 也可用分布来模拟随机漫步

> 一些tips: 转换为对角阵，使用乘法来做
1. 对列求和, 然后做每行；直接 T / np.sum(T, axis=1, keepdims=True) // 保证最后得出的结果是矩阵
2. np.mat(A): 返回矩阵的抽象类型，什么时候用到也不知道
