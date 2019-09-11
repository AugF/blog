---
title: deep learning study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-10 20:56:16
tags:
- deeplearning
- tools
- numpy
- tensorflow
categories:
- [tools, tensorflow]
---

## 1. tensorflows

tensorflow 数据流图  用于数值计算单开源软件库

Nodes : 数学操作
> 特别地，有数据输入和数据输出的节点， 或者是读取、写入持久变量的终点
> persistent

edges: 多维数据数组， tensor
> 根据不同的计算, 张, 即数据的大小可变size

特征
- 高度的灵活性，可自己写C++
- 可以在CPU, GPU, Docker等， Portablilty 可移植性
- 科研和产品
- 自动求微分
> 只需要添加数据和目标函数，可以自动计算相关的微分导数
- 多语言支持
- 支持线程、队列、异步操作的支持，可以自由第将TensorFlow中的计算元素分配到不同的设备中

<!-- more -->
### 1.1 什么是tensorflow

能够在几行代码之内构建模型，能够在几十或几百个机器的簇是哪个进行训练，从而用该模型进行非常低的延迟预测

模型会以图的形式展现出来，可以推迟或删除不必要的操作，甚至重用部分结果，还可以很轻松地实现的一个过程叫做反向传播。
基于所见的样本以及计算的误差。来更新模型中的链接强度，这整个过程就是反向传播

因为模型表现为操作图而不是代码，因此会自动地计算以及应用这些迭代更新。

可以用一行代码声明，我想这部分图在这里运行，另一部分分布式运行在不同的机器上。

注重数学的在GPU上运行，与此同时，数据输入的代码在CPU运行。专门的硬件TPU

tensorflow可以在ios安卓、甚至树莓派等设备上加载。 

TensorBoard可视化哦工具

### 1.2 tensorflow可以做什么

谷歌在多种应用上应用了tensorflow

### 1.3 tensorflow入门指南

- 边: Tensor
    > 把所所有都看作张量
    > 属性
        - 数据类型 tf.float32,  tf.complex32 ? 怎么表示, tf.int32,  tf.string, tf.bool
        - 形状 list:  [1,2,3,4]
    > 操作
        - 获取阶  tf_tensor1_ndim = tf.rank(tf_tensor1)
        - 切片  tensor[:,1] ?是否是引用
        - 返回numpy数组  tf.eval(tensor)
    > 分类
    - 每次运行都一样
        - 1维 tf.constant(tf.type)
    - 每次运行都不一样 
       > 占位input_node = tf.placeholder(tf.int32) + 输入 sees.run( , feed_dict={input_node: 2})
    - 每次运行中都发生变化，比如模型中的参数
        > 如果设置变量
        - tf_var = tf.get_variable(name, shape=, initializer=tf.costant())
            - name唯一标志，不可重复，下文可直接使用？一般两个名字一样？
                > 理解: name主要有两个用法：1.保存时可以直接使用变量保存  2. tensorboard时用来使用
                > why? 因为这里name实际上是tensorflow中自己存储的命名空间，而py文件中的名字是python中的命名空间，所以当脚本运行终止后将不复存在
            - shape:[1,2],  []表示标量
            - initializer: 默认值  arry-like
                > 如果默认所有变量都使用默认值启动，那么tf.global_variables_initializer()
                > 当发生变化时，判断是否合理
                - 取值
                    - tf.constant
                    - tf.zeros_initializer,  tf.glorot_uniform_initializer
            - 变量检查 assign_node = tf.assign(tf_var, default_node)  default_node = tf.constan(arr) 为true表示成功
            - 运行时靠什么启动? sess.run(assign_node), sess.run(tf.var)
        - tf_var = tf.Variable(initializer=arr_like_list, tf.type, name=, traninable=Fals.e, collections=[])  collections表示可能的类型
            > 获取值  sess.run(tf_var.initializer)   sess.run(tf_var)
        - 进阶：考虑作用域问题，将问题转换为作用域问题
            > 用途主要在RNN循环神经网络的构建中会使用到
            with tf.variable_scope(scope_name, reuse=False):
                v1 = get_variable('x', [1])
            > 技巧： 把每个层看作一个单元 scope_name为str, 直接进行改变名称即可; 当指定reuse=True时，可以进行变量复用

- 节点： operation

- 架构
    - sess = tf.Session()
        - sess.run(args)
            - args(0): list(nodes)  需要输出的节点
            - feed_dict={} : 为前面tf.placeholder(tf.int32)占位的数据输入数据
    - 机器学习过程
        - 设计抽象模型
            - 构建网络结构 x= tf.placeholder  y=tf.placeholder m=tf.var  b=tf.var 
            - 模型表达式 y_guess = mx + b
            - 损失定义 loss = tf.square(y-y_guess)
            - 指定梯度下降方法 optimizer = tf.train.GradientDescentOptimizer(lambda): 学习率
            - train_op = optimizer.minimize(loss)  训练模型
        - 开始训练
            - loss, _ = sess.run([loss, train_op], feed_dict={input_placehold: ,  output_placehold: }) 
            > _占位, 表示train_op.
            > 这样直接完成一轮训练？并没有说迭代终止条件啊！！！
            > input_placeholder应该是支持数组的吧？ 还有应该默认就把var看作模型的参数，每次梯度下降进行更新吧？每个变量tf.Variable.init()中有可选方法， traninable来选择是否使用到Optimizer, 不过这应该是不会在模型中使用到吧
        - debug
            - print_sum_node = tf.Print(node1,list(nodes))  // 表示自己想要的node

### 1.4 其它

tensorflow使用数据流图将计算表示为独立的指令之间的依赖关系，这可生成低级别的编程模型。
首先定义低级别的编程模型，在改模型中，首先定义数据流图，然后创建tensorflow会话。

why？ 数据流图
> 数据流是一种用于并行计算的常用模型。tf.matmul操作对应于单个节点
- 并行处理。通过使用明确的边缘来表示操作之间的依赖关系
- 分布式执行。通过使用明确的边缘来表示操作之间流动的值，可以将程序划分到不同机器的多种平台
- 编译. XLA编译器可以使用数据流图中的信息生成更快的代码，例如将相邻的操作融合到一起
- 可移植性。不依赖与模型的代码表示法

tf.Graph结构
- 图结构：图的节点和边缘，表示各个操作组合在一起的方式，但不规定它们的使用方式
    - tf.Operation: 节点. 
        - tf.Variable(0)
        - tf.matul(x,y)
        - tf.constant(42,0)
        - tf.train.Optimizer.minmize(): 执行的最后，该操作在执行时将返回一个tf.Operation. 该操作在运行时将这些梯度用到一组变量上。
        > 一般地，直到拥有表示整个计算（例如梯度下降法的一步）的tf.Tensor或tf.Operation,才使用Session计算
- 图集合: 在图中存储了元数据集合的通用机制。
    > 如Variable, collections, 分全局变量和可训练变量

？如何保存整个图，保存整个图的关键就在于保存节点和边，所以在tensorflow中就是如何保留节点和边。
即Tensor, Operation
> 不保留Operation, 每个Tensor都有自己的名称。特别地是，在保存时，Tensor命名为"生成它的操作：索引"？？？, 是否有name

- 将操作放置到不同的设备
    - 设备规范：`/job:<JOB-NAME>/task:<TASK-INDEX>/device:<DEVICE_TYPE>:<DEVICE_INDEX>`
        > 其中device_type即决定CPU还是GPU
    - 并行化？
        - 这里的并行化从编程的角度的感觉就是指定每个device安排哪些工作而已。
        > ? 总的操作应该是合起来看的吧？
            - 任务并行
                ```
                with tf.device("/job:ps/task:0"):
                    weights_2 = tf.Variable(tf.truncated_normal[]) 
                with tf.device("/job:ps/task:1"):
                    weights_1 = 
                ```
            - 数据并行
                ```
                with tf.device(tf.train.replica_device_setter(ps_task=3)):
                    w_0 = tf.Varible  // task:0
                    b_0 = tf.Varible  // task:1
                ```

- 类张量对象
    - tf.Tensor
    - tf.Varible
    - numpy.ndarray
    - list
    - 标量Python数据类型
    > 注意！在写代码时最好将所有的类张量都转换为tf.Tensor. 否则，每次使用时都会创建新的Tensor, 会爆内存

- 会话 tf.Session
    > 推荐 `with tf.Session() as sess: #..`
    > 会话是拥有物理资源如GPU和网络连接的
    > tf.train.MonitoredTrainingSession或tf.estimatorEstimator? 将会自动创建和管理Session
    - tf.Session.init三个参数
        - target: 默认将使用本地机器中的设备，也可以指定棋子。
        - graph: 会默认捆绑到当前的图
        - config: 配置设备、机器以及优化
    - tf.Session.run(): 执行动作，会强调需要执行哪些子图. 在执行最终节点之前，务必注意需要将所有的变量的初始化操作完成
        - fetches: list, 指定必须要执行哪些操作
        - feed_dict: dict为图的元素赋予值
        - options
        - run_metadate

- 直观展示图
    - tf.summary.FileWriter(save_path, sess.graph): 在with tf.Session() as sess之后，即将图已经建好之后
    > 发现一个事情？ sess.run在写代码时，感觉到一个点就是，每个点是单独算的。然后进行梯度更新的，然后图神经网络是这样没有问题的，但是一般的算法也是这样搞的吗？
- 保存和恢复
    - 变量保存
        saver = tf.train.Saver(),  saver.save(sess, "model.ckpt");    tf.reset_default_graph()   saver.restore(sess, "model.ckpt")
        > 注意这里的保存方式就是之前讨论的保存方式。然后还有一点就是默认会保存全部的变量 checkpoint file
        > 特殊定制化： saver.save(var_name, var)
    - 模型保存
        tf.saved_model.simple_save

- debug 待扩展

- 性能
    - 输入流水线
    > 输入流水线会从一个位置提取数据，对数据进行转换，然后将数据加载到加速器上进行处理。
    > https://zhuanlan.zhihu.com/p/43356309 推荐大数据100MB以上使用tf.data,小数据集使用feed_dict
        - create `.tfrecords` file,  思路即通过迭代器来做
        - read '.rfrecords` as input, 整个过程使用迭代器来完成 
    - RNN: 动态和静态的RNN, 会将内存从GPU切换到CPU
    - 针对CPU预处理: 配置线程数或者编译器版本mkg
### 1.5 tensorboard
tensor神经网络可视化
> 有多个可视化的点
https://zhuanlan.zhihu.com/p/36946874

### 1.6 keras 高级接口

训练神经网络中最基础的三个概念：
1. epoch: 使用训练集的全部数据对模型进行一次完整训练，成为一代训练
2. batch: 使用训练集中的一小部分样本对模型权重进行一次反向传播的参数更新，这一小部分样本称为一批数据
3. Iteration: 使用一个Batch数据对模型进行一次参数更新的过程，称为一次训练

tf.keras高阶API
    - 方便使用
    - 模块化和可组合
    - 易于扩展

包括特定功能
    - Eager Execution
    - tf.data
    - Estimator

序列模型：这里说的就是搭积木的意思，即自己一层一层地建立神经网络的连接
```
mode = tf.keras.Sequential()  
// 线性模型，这里，那么后面的Dense中的参数64是怎么是定的
mode.add(layers.Dense(64, activation="relu"))
mode.add(layers.Dense(64, activation="relu))
```

tf.keras.layer
- activation: 设置层的激活函数。此参数由内置函数的名称指定 "relu",  tf.relu
- kernel_initializer 和bias_initializer: 创建层权重（核和偏差）的初始化方案。默认"Glorot uniform"
- kernel_regularizer和biar_regularizer: 应用层权重的正则化方案，例如L1或L2正则化


keras文档

> https://blog.csdn.net/sinat_26917383/article/details/72857454

具体思路：
Dense这里不过表示深度学习中某一层的全连接层而已

https://blog.csdn.net/sinat_26917383/article/details/72857454
https://tensorflow.org/guide/keras

下一步，探讨需要什么环境？

RNN是涉及到具体的链接层的。这里的层是进行抽象的，keras
tf.Dense():  应该是没有参数定义实际上应该怎么运算？ 也就是说对于图神经网络的结构只能用tensorflow底层接口来做。

#### 1) 一个完整的pipeline
1. 构建简单的模型
    - 序列模型
        - 两种初始化方法
            - model = Sequential() model.add(layer)
            - model = Sequential([layer1, layer2, ..])
    - 配置层
        > layer即配置层的含义，在这里可以指多个层。对于深度神经网络而言，dropout的实现不过就是在需要的地方加上一层dropout
        - Dense配置的东西  https://www.tensorflow.org/api_docs/python/tf/keras/layers/Dense
            > Dense(32, input_shape(16, ))  # input array of shape (*, 16); output arrays of shape (*, 32)  一般*为batch_size的大小
            - units: dimensionality of the output space
            - activation: 激活函数
            - user_bias: boolean,  是否使用bias向量 ??? 意思是输入是向量，而不是矩阵？？
            - initializer:  kernel,  bias
            - regularize: kernel, bias
            - constraint: kernel, bias
        - 关于layer的配置,
2. 训练和评估
    - model.compile()
        - optimizer: tf.train.AdamOptimizer, tf.train.RMSPropOptimizerm tf.GradientDescentOptimizer
        - loss, 在优化期间最小化的函数，常见均方误差mse,, categorical_crossentropy  binary_crossentropy
        - metrics, 用于监控训练，字符串或者可调用对象,  比如准确率
    - mode.fit()
        - data
        - labels
        - epochs: 总共运行几次训练
        - batch_size: 一次训练使用多少个样本
        - validation_split: 0~1, 交叉验证集占训练集的比例
        - validation_data = (val_data, val_labels)), 与上面的二选其1进行
        > 该命令执行后直接会运行
    - model.evaluate(data, labels, batch_size=32) 对未知数据集和预测结果进行评价   // 每个batch_size为一个评价单元
    - model.predict(data, batch_size)  模型预测

#### 2）构建高级模型
##### 函数式API
- 多输入
- 多输出
- 具有共享层(同一层被调用多次)
- 具有非序列数据流的模型（剩余连接）

```
inputs = tf.keras.Input(shape=(32,))  # Returns a placeholder tensor

# A layer instance is callable on a tensor, and returns a tensor.
x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dense(64, activation='relu')(x)
predictions = layers.Dense(10, activation='softmax')(x)

model = tf.keras.Model(inputs=inputs, outputs=predictions)

# The compile step specifies the training configuration.
model.compile(optimizer=tf.train.RMSPropOptimizer(0.001),
              loss='categorical_crossentropy',
              metrics=['accuracy'])

# Trains for 5 epochs
model.fit(data, labels, batch_size=32, epochs=5)
```
> 使用了函数式编程接口，tensor为输入变量，返回结果也为tensor

##### 模型子类化

> 通过对tf.keras.Model进行子类化并定义自己的前向传播来构建完全可自定义的模型。在__init__中创建层并将它们设置为类实例的属性。在call方法中定义前向传播
> 通过继承keras中已有Model, 还是根据本身已有的数据，还是调用原本的功能，不过会进行添加一些新功能

##### 自定义层
tf.keras.layer.Layer进行子类化并实现以下方法自定义层
只需要实现以下接口
- build: 创建层的权重 add_weight方法添加权重
    > shape = tf.TensorShape()  self.kernel = self.add_weight()  super(MyLayer, self).builf(input_shape)
- call: 定义前向传播
    > return tf.matmul(inputs, self.kernel)
- compute_output_shape: 指定在给出形状的情况下如何计算层的输出相撞
    > shape = tf.TensorShape(input_shape).as_list()
    > shape[-1]=self.output_dim
    return tf.TensorShape(shape)
- 或者，实现get_config方法序列化层, from_config;  因为需要涉及到存储
    > base_config = super(MyLayer, self).get_config()
    > base_config['output_dim']=self.output_dim
    > return base_conofig
    > @classmethod
    > def from_config(cls, config): return cls(**config)

？？ python 如何进行继承，是方法抽象吗？ 还有抽象方法呢？
@关键字
全局变量呢
## 2. numpy

https://www.numpy.org.cn/

> python 太慢！！有个点就是关于python的切片都是复制，不是引用！不同numpy
> numpy基于C直接操作连续内存空间，比纯Python快10到100倍，并且使用内存更少

numpy提出的ndarry很好用,主要理解通用的数值数据处理上的强大功能，特别是在数组操作上
pandas主要用于统计和分析

！！！ 如何用整个数组的思想
> %time 可以直接统计时间

### 2.1 ndarry

#### 2.1.1 属性与方法

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

2. np.mat(A): 返回举证的特殊的抽象类型，什么时候用到也不知道

## 3. pandas
https://www.pypandas.cn/

pandas: 处理表格和混杂数据
numpy: 处理统一的数值数据
scipy: 数值计算工具
分析库：scikit-learn 和 statsmodels
数据化可视库: matplotlib

- 数据结构
    - Series: 类型与一维数组的对象，它有一组数据（各种numpy数据类型）以及一组一直相关的数据标签即索引组成。
        > 索引可以为字符串，在使用numpy函数或者类似num朋友的运算，如布尔型数组的过滤、标量乘法、应用数学等都会保留索引值的链接
        > np.exp(Series),  即可以直接传入series
        > Series可以看作是一个定长的**有序**字典，因为它是索引值到数据值的一个映射, 索引是key, 可以用in
        - init初始化: 深复制
            - dict: ser1 = pd.Series(dict)
            - list: ser2 = pd.Series(list1, index=list2)
            - array: ser2 = pd.Series(arr1, index=list1) 
        - index：  list('abcdef') // 超级简单的方法
            - index本身就是属性,  RangeIndex, Index两种
            - ser2.index=list3 // 直接修改
    - DataFrame
        > 表格性的数据结构，它包含一组有序的列，每列可以是不同的值类型（数值、字符串、布尔值等）。DataFrame即有行索引，也有列索引。
        > 可以看作是由Series组成的字典，共用同一索引
        > Dataframe中的数据是以一个或多个二维快存放的。 虽然是二维结构保存数据的，但可以很轻松地表示为更高维度的数据
        - 属性
            - df1.index.name / df1.columns.name
            - df1.values
            - df1.index: 不可改变，切片也不可改变；为不可变对象; Index类型,  因为是序列，所以允许重复元素
                > 一些操作
                - append, difference,  intersection, union, sin, delete, drop, insert(i, ) isunique, unique
        - init
            - dict: df1 = pd.DataFrame(dict):  dict.key是等长的list, 将key映射为列名；列顺序未被指定
                > 注意元组会被当成一个值
            - 2d_arr:  df1 = pd.DataFrame(2d_arr, columns=list1, index=list2) // 注意columns是指定列名， 并且指定了列顺序; index指定行名
        - 索引： 一旦给定就确定下来，不会发生任何改变
            - Series: df1[column_name], df.column_name
            - Value: df1[column_name][index_name]
            - Object行,  df1.loc[index_name]  
        - 添加、修改
            > 按照类型赋值
            - 按索引的方式添加，添加等于修改
                - df1[column_name] = Series: 不够的值，没有的值会赋np.NaN
                - df1.loc[index_name]=list1: 必须匹配，否则报错
                - df1[colume_name][index_name]=1
        - 删除
            - 删除列 del df1[column_name]
            - 删除全部 del df1
            - 批量删除 drop
                - df1.drop(colums_list)
                - df1.drop(index_list)
                - df1.drop(list1, axis): 按默认来，0为x轴 
        - 取切片的都属于浅复制, 即改变任何一个的元素都会引起另一个的改变。
            > 注意这里是末端包含式切片
            > 特别地, 当删除frame中的serie时，新的serie还存在, 为变化之前的值
            > `ser1 = pd1['value'],   pd1['value'][2]=1, del dp1['value'];  ser1仍存在`
            - 杂项
                - 范围 pd1[columnN: columnM]: 因为是序列数据，所以直接取范围; 可用int
                - 特定项 pd1[[column1, colum3, colum4]]， Series可用int做, 默认从0开始
                - obj[cond]
            - 通法
                - pd1.loc[val, val2] 标签
                - pd1.iloc[i, j] 整数
        - 特殊值处理
            - 空值 "" isnull()
            - 缺失值 nan或naT  isna()
            - 两种手段
                - 删除对应行和列： df.dropna()
                - 填充 df.dropna()  // 细粒度地填充, inplace=True在原来上操作   

- 基本功能
    - 重新索引
        - row  ser1.reindex(new_index, method="ffill") // 使用前向值进行填充; fill_value: 替代缺失值; limit最大填充量
        - columns pd.reindex(columns=new_columns)
    - 算术运算和数据对齐
        > 必须行名，列名相同才好使
        > 非对齐使用nan填充
        - 会用到吗？
            - add, radd: r表示reverse版本，即将两个操作数的位置交换
            - sub, rsub
            - div, rdiv
            - floordiv, rfloordiv
            - mul, rmul
            - pow, rpow
    - DateFrame和Series: 广播运算，对每一行都做
    - pd1.apply(func, args=): 特别好用 + lambda表达式，作用在每个元素级别
    - 排序和排名
        - sort by index: pd.sort_index(axis=0)
        - sort by value
            - ser1.sort_value() //nan在尾部
            - pd1.sort_value(by=[col1,col2])
        - rank
            obj.rank(ascending=false, axis= , method= )
            - method:  相同排名的处理问题;  first 按索引的第一个;  max, 最大; min; average
    - 汇总和计算描述统计->基于列的Series
        - df.sum(axis=,  skipna=)
        - df.idxmax() 索引位置  max() 索引值
        - df.cumsum()
        > count, mean, std, min, max
    - 唯一值，值计算及成员资格
        - ser1.unique()
        - ser1.value_counts() // 计算重复出现的值
        - ser1.isin(ser2)
        - ser1.match(ser2) 对齐数据, 比较返回真假值

- 数据加载、存储和文件格式
    - 读写
        - read_csv( seq=',')
        - read_table(seq='\t')
    - 处理
        - 索引
        - 类型推断和数据转换
        - 日期解析
        - 迭代
        - 不规整数据问题

## 4. matplotlib

matplotlib
更详细的内容
https://www.matplotlib.org.cn/api/overview/index.html

- 基本API入门 `import matplotlib.pyplot as plt`
    > 只要返回对象是figure的，就会在jupyter中产生图
    > 在py文件中，则是使用plt.show()产生图
    - 多个子图
        - fig = plt.figure()
            - ax1 = fig.add_subplot(2,2,1)  //(length, width, rank) rank从1开始
            > 此时ax1 就可用作 plt
        - fig, axs = plt.subplots(2,3)
            > axs[0,1]获取子图
            - plt.subplots(args) 参数
                - nrows,  ncols
                - sharex, sharey 是否共享x，y轴
                - subplot_kw 各subplot的关键字字典
            - fig.subplots_adjust(wspace=0.3,  hspace=0.5)
                > 这里可以直接使用plt, 因为matplotlib会自动引用新添加的子组件的对象
                - wspace, hspace, 图与图之间的间隙中宽度为子图的多少倍, 即一般为0.3之类, 为百分比
    - 图的类型
        - plot(x[,y])   折线图，x默认索引
        - hist(y)  条形图，x默认索引
        - scatter(x,y) 散点图
    - 操作图
        - 调整子图周围的间距 wspace,  hspace
        - plot(args) 参数
            - fmt_str "{marker}{line}{color}" 或者 `[color][marker][line]`
                - marker
                    - `.`:  point
                    - `o`: 实心circle
                    - `v`, `^`, `>`, `<`: 各个方位的实心三角形
                    - 1, 2, 3, 4: 箭头，下、上、左、右
                    - |,_: vline, hline
                    - s: square;  *: start;  D: diamond,; p: 多边形; +,x
                - linestyle
                    - `-`: 实线
                    - `--`:虚线
                    - `-.`: 非规整虚线
                    - `:`: 小点小点的线
                - color
                    - b: blue, g:green, r:red,  c:cyan, y:yellow, k:black, w:white
            - lable: 设置图标说明
            - drawstyle: 点到点的其它路径画法
            - xlim, yli: x,y的界限
            - grid: 网格线是否打开
        - set方式设置参数
            > plt.set(dict) 全局设置
            > plt.legend() 用于增加图裂
            - 设置刻度标签，为刻度赋予自己的意义
                > tricks = ax.set_xtricks([0,250,500,750,1000]); 
                > labels = ax.set_xticklabels(list('abcdf'))
            - 图的标题 title
                > ax.set_titile("")
            - 图的x轴lable
                > ax.set_xlabel()
            - 图解，标注某个点
                > ax.annotate(label, xy=(x,y))
            - 保存文件
                > plt.savefig(".png")
    - 全局配置，字体什么的  plt.rc();    pandas本身ser.plot(),  df.plot()就可以进行绘图

- 应用
    - 画等高线图
        1. 取特定个点  np.linspace(start, end, number),  np.arange(start, end, step)
        2. 画网格   xx, yy=np.meshgrid(x,y)  xx表示n个x轴的数据  yy表示m个y轴的数据
        3. plt.contour(x,y,f(x,y)) // 绘制等高线
    - 动画animate
        - animation.FuncAnimation(fig=fig, func=animate, init_func=, frames=length, interval=60ms频率, blit=True是否更新所有的点)
            > line.set_data([],[])    point.set_data([],[])
            - def animation(i):  line.set_ydata((np.sin(x+i/100)))
    
## 5. scipy

scipy库依赖于numpy库，它提供了便捷且快速的N维数组操作。构建scipy库的原因是，它能与numpy数组一起工作，并提供许多用户友好和高效的数字实践，例如数值积分和优化的教程
numerical integration, interpolation插值， optimization, linear algebra线性代数 statistics统计

- cluster： cluster algorithms
    - vq: 只支持矢量化 vector quantization, k-means algorithms
        - whiten: normalize a group of observations on a per feature basis
        - vq: assign codes from a code book to observations
        - kmeans: 
    - hierarchy: hierarchical and agglomerative clustering, 支持层次聚类和凝聚聚类; 1. generating hierarchical clusters from distance matrices 2. calculating statistics on clusters 3. cutting linkages to generate flat clusters 4. visualizing clusters with dendrograms
- constants: physcial and mathematical constants
    - math
        - pi
        - golden
        - golden_ratio
    - physical
        - c: 光速
    - quick getting:  constants.value(u"elementary charge"), unit, precision, find
- fftpack: fast fourier transform routines
    - fft: discrete 离散, both real or complex sequence
    - ifft: inverse相反
    - fft2, ifft2: 2-d
    - fftn, ifftn
    - dct 余弦变换
    - dst: discrete sine transform
- integrate: integration and orginary differential equation slovers
    - quad: definite integral: 有限积分，三个参数，函数、上下限
    - dblquad: dobule类型
    - tplquad: 计算多重积分的表达式, 积分限是函数
    - nquad: 多重积分，积分限是具体的值
    - fixed_quad: 计算一个固定的积分使用fixed-order的高斯分布
    - trapz: 使用复合梯度算法计算, 同理simpz, romb
- interpolate: 插值, interpolation and smoothing splines
    - interp1d
- io: input and output
    > loadmat, savemat,可以加载matlab文件
- linalg: linear algebra
    - basic
        - inv
        - solve(a,b)  a*x=b
        - solve_banded(): a是带状矩阵
        - solve_triangular(a,b): 解决三角举证
        - det 行列式
        - norm 范数
        - lstsq() 二乘解
        - pinv: 矩阵的伪逆
        - kron: 元素乘积
    - eigenvalue problems: 特征值问题
        - eig: 方阵特征值问题
    - Decompositions
        - lu: LU分解
        - svd： 奇异值分解
        - qr：QR分解
    - Matrix Functions
        - expm: 使用近似方法
    - Matrix Equation Solvers
- ndimage: n-dimensional image processing
    - convolve(): 多维卷积操作
    - gaussian_filter(): 高斯过滤
    - laplace： 拉普拉斯过滤
    - floutier_ellipsoid: 多维ellipsoid fourier filter
- odr: orthogoal distance regression 正交距离回归问题
- optimize: optimization and root-finding routines
    - Optimization
        - scalar function标量函数: minimize_scalar
        - Local(Multivariate) Optimization: 多元回归
        - Global Optimization: brute()
        - Least-squares and Curve Fitting
            - nonlinear
            - linear
            - curve fitting
        - Root finding
    - Linear Programming
        - linprog
    - Utilities
        - Finite-Difference Approximation
            - approx_fprime
            - check_grad
        - Line Search
        - Hessian Approximation
        - Benchmark Problems
    - Legacy Fuctions 遗留功能
        - fmin
- signal: signal processing
    - convolution
        - convolve
    - B-splines
        - bspline, cubic
    - Filtering
    - Filter design
    - Matlab-style IIR filter design: butter, cheby1
    - Continuous-Time Linear Systems: lti()
    - Discrete-Time Linear Systems
    - LTI Representations
    - Waveforms
    - Window functions
    - Wavelets
    - Peak finding
    - Spectral Analysis
- sparse: sparse matrices and associated routines
    - Contents
        - bsr_matrix: block sparse row matrix
        - coo_matrix: coordinate format
        - csr_matrix,, csc_matrix: compressed sparse row matrix
        - dia_matrix: sparse matrix with disgonal storage
        - dok_matrix: dictionary of keys based sparse matrix
        - lil_matrix: row-based linked list sparse matrix
        - spmatrix: 以上的所有类型
    - Functions
        - eye(): ones on diagonal
        - identity()
        - kron(A,B)
        - kronsum
        - diags(), spdiags
        - block_diag
        - tril
        - bmat: build,  hstack, vstack
        - rand: uniformly distributed values.  random()
    - Save and load sparse matrices
    - Sparse matrix tools: find(A)
    - identifying sparse matrices: issparse(x)
- spatial: spatial data structures and algorithms
    - Spatial Transformations
    - Nearest-neighbor Queries
        - KDTree
        - ckDTree
        - Rectangle: Hyperrectangel
    - Delaunay Triangulation, Convex Hulls and Voronoi Diagrams
    - Plotting Helpers
    - Simplex representation
- special special functions
- stats: statistical distributions and function
    - Statistical functions
    - Continuous distributions
    - Multivariate distributions
    - Discrete distributions
    - Summary statistics: descrube, gmean
    - Frequency statistics: cumfreq
    - Correlation functions
    - Statistical tests
    - Transformations


python: help in the pydoc module
help(obj)
numpy.info(obj)

