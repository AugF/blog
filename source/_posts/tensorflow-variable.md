---
title: tensorflow variable
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-24 16:23:33
tags:
- deeplearning
- tensorflow
categories:
- [tools, tensorflow]
---
https://blog.csdn.net/shenxiaolu1984/article/details/52815641
> tensorflow变量相关

https://blog.csdn.net/huqinweI987/article/details/82771521
> tensorflow自动训练简介和选择训练

## 概览
tf.Optimizer只优化tf.GraphKeys.TRAINABLE_VARIABLES变量

init=tf.initialize_all_variables()
sess.run(init)  // 初始化之后Variable中的值生成完毕，不再变化

v = tf.all_variables() // 查看所有tf.GraphKeys.VARIABLES
v = tf.get_collection(tf.GraphKeys.VARIABLES)

## 各类Variable
- 获取训练的变量 方式1
    - tf.all_variables()
        > 直接在某种状态下进行运行即可
    - tf.trainable_variables()
- 获取训练的变量 方式2
    - tf.get_collection(tf.GraphKeys.TRAINABLE_VARIABLES)
    - 优化特定的变量:  optimizer.minimize(cost, var_list)

sess: how to get a default sess

tf.gradients()
> 需要注意的是grad_weights这里返回得到的是list

使用一个未知的函数时，请务必详细地查看参数

想想tensorflow, 真的是厉害， 把模型具体给抽象出来


做事之前先想清楚，比如说对这段时间科研进行总结的话

一件事情不要测试过多，过于复杂

首要目标应该是把事情做完。

而且遇到挫折，不要扔，可以暂时转移，应该是想如何解决问题

ctrl+shift+f 全局搜索

一定要精确度到最细

而且要把测试代码单独拿出来，没做一次改变进行一次测试

git 使用中扩展思路总结:
1. 代码在当前版本，然后进行两部分分别测试，那么可以考虑创建两个分支
2. 代码提交的版本最好是能够运行的

