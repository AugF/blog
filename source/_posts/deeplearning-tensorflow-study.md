---
title: deeplearning tensorflow study
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:22:41
tags:
- tensorflow
- python
categories:
- [tools, tensorflow]
---

# 一个pipeline
> 一层封装，但还是仍然比较繁琐
https://blog.csdn.net/u012896627/article/details/72874678

1. 建立一个sess=tf.Session
    > 交互式会话跟全局会话有什么区别
    > tf.Session.init()
    - tf.Session vs tf.InteractiveSession
    > 原本每个张量的计算，以及最后的取值都需要在默认的Session中，而且最终的运行也离不开Session.run(); 采用InteractiveSession后， 本身就为默认的session, 此时距直接tensor.eval() 即可运行， vai.init.run()即可运行
2. 中间涉及到的张量Tensor, 主要相关的其实就是矩阵运算
    - tensor类型
        - tf.placeholder()  // 通常用于小数据集的输入, 在后面的feed_back={} 将进行制定
        - tf.Variable(name=, intilizer=)  // 可用于梯度 更新的内容 
        - tf.constant(name= , shape=)
    - tensor初始化
        - 获取指定分布的值
            - random_normal 正态分布 random_uniform 聚云分布 truncated_normal
    - 区分不同作用域的变量
        - with tf.variable_scope(scope_name, resue=):  v1=""
    - 小技巧
        - tensor.eval() // 查看tensor的numpy矩阵
        - tensor.shape  // 查看形状
        - v.assign()  assign(  )
        - tf.get_variable(name): 获取指定名称的var
3. Tensor之间的运算, 即边
    > https://blog.csdn.net/u012896627/article/details/72874678
    - 矩阵变换, 这个本身可以用numpy来做？
        - tf.expands_dims(arr, 1), 在指定的轴， 比如axis=1，增加一维，原数据不变？ 如其名，
        - tf.concat(arr1, arr2, axis=1) 在某一维上将两个向量合并。 (2,3)->(2,6), (4,3) 发现对于0来说是倒着来的
        - tf.sparse_to_dense()
        - tf.random_shuffle()  按照axis=0, 进行制定shuffle
        - tf.argmax/ arg.min 找到最大最小所对应的的位置
        - tf.equal 判断两个tensor是否每个元素都相等，返回bool的tensor
        - tf.cast 将x的数据格式转化为dtype
        - tf.matul 用来做举证乘法
    - 神经网络
        - tf.nn.embedding_lookup 将一个数字序列转化为embedding序列表示？举证
        - tf.trainable_variables 返回多个有训练的变量
        - tf.gradients 用来计算导数
        - tf.nn.dropout 防止过拟合
    - 普通操作
        - tf.linrange()  tf.range()
    - 规范化
        - tf.variable_scope
        - tf.getvariale_scope
    - 
4. 变量的初始化操作
    - Session.run(vari.initilizer)  // 进行变量的初始化
    - tf.initialize_all_variables().run()
5. 损失函数和梯度下降
    - 损失函数为图的最后一个tensor
    - 优化器. optimizer = tf.train.GradientDesecentOptimizer(0.5)
    - train = ptimizer.minimize(cross_entropy)
6. tensorflow运行
    - sess.run(fetches, feed_dict,  options)  // 
        - fetches: tensor_node
        - feed_dict
        - return: fetches中的值
7. 预测
    - tf.equal(tf.argmax(a, 1), tf.argmax(g,1))
    - accuary = tf.reduce_mean()
8. tensor.close()

> 另外，所谓的tf.Graph; 每个tensor都可以产生一个graph; 或者定义一个graph, 之后都在该作用域下进行声明tensor 



tf快捷配置命令行参数
> 并不是技术，用在了很多方面
```
// 声明
FLAGS = tf.app.flags.FLAGS
tf.app.flags.DEFINE_string(props_str, default_str) // DEFINE_TYPE

// 使用
FLAGS.props
```

- 查看GPU
nvidia-smi

- 以后计划任务，务必注意查看需要花费的时间


什么是skip-gram算法？
 skip-gram算法是什么，在给出目标单词的情况下，预测它的上下文单词，这里会给定指定窗口大小。
 思想：https://zhuanlan.zhihu.com/p/29305464
 > 没看懂，还是没明白第一个矩阵W是V*d, 是当前矩阵的什么。还有就是用到邻接矩阵的向量？？

https://zhuanlan.zhihu.com/p/27234078


扩展，一点想法，代理是什么？
代理服务，代理协议，代理端口
首先代理服务，有邮件
代理协议就是像常见的http/https协议等等
代理端口也就是所用的代理端口的感觉

两个问题：现在已经测试了可以开启代理服务器的方式允许各种方式的网络配置。
已经测试了同局域网下可以访问，如何允许私网段以及不同局域网访问呢？按理说是能做到的

ssh连接远程访问也可以通过代理服务器来做，也就是说如果解决了不同局域网的访问，那么就可以访问到集群

很多配置相关的文件都在~/.ssh下

现在所配置的代理不过都是私网地址，即局域网内允许使用的地址
另外有一种情况就是NAT，地址映射协议，由某一地址统一管理。如果其他计算机需要访问的话，需要首先在主节点上进行说明。

- docker是什么？有什么用？
相关概念：虚拟机，操作系统
http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html
虚拟机是在一个操作系统之上再嵌套一个操作系统。

对于虚拟机来说，它管理很多方面，有着很多方面的高效性。就是一个操作系统，并且能够提供各种服务，比如文件服务、网络

而docker是进行进程的模拟，所以提供的服务会很快。启动快，占用资源少
有什么用？
提供一次性的环境， 提供弹性的云服务， 组件微服务架