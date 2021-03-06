---
title: gnn-parallel-gnn-overview-paper-summary
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:59:16
tags:
- gnn-parallel
- gnn
- paper
categories:
- [gnn-parallel, gnn]
---
[TOC]
# 1. 序言
图有着复杂的关系和交互依赖

2D卷积: 邻居的决定由filter size
w = 平均， 邻居有序，可以按照一定的序列来看且有固定的尺寸

Graph卷积： 邻居无序，价值等同，数量不一

Random Walk??? 随机游走，有概率计算方法还有要理解？

图网络表示：
1. 编码
2. 随机游走
3. 矩阵因式分解

# 2. 框架

Node-level:  MLP+softmax: 见pooling结构
Edge-level: 额外的函数将两个节点的表示作为边
Graph-level: 就是pooling

半监督: 只能适用于node-level
监督学习: 适用于graph-level
无监督： graph-embedding
> 编码器+译码器  负采样
     


# 3. 正文
## 3.1 GCN: 

? stack多层可以学到更多的东西. 对！注意这里的输入是整个图
如果堆叠多层的话，实际上就可以不断的获得更远的信息

basic
> 这里每个的输出应该不会只是一个值吧？？
Relu修正线性函数：
当大于x时为本身，小于x时为0

<font color="red">ReLU</font>
ReLU可以将模型实现稀疏话更好地挖掘相关特征
就激活函数而言，在深度网络中，由于非负区间的梯度为常数，所以不存在梯度消失问题，使得模型的收敛速度维持在一个稳定状态。
https://blog.csdn.net/cherrylvlei/article/details/53149381

pooling

一个图经过GCN加pooling得到一个子图，然后给下一个模型。如此最后不断地得到更高级别的表示，最后一层是MLP + softmax

<font color="red">MLP</font>
MLP: 多层感知机 vs 支持向量机 SVM
> 多层感知机其实就是多层网络结构，但是一般是向前的，不会有后退的，然后关键就是设定一个偏差评价函数，以及某种策略，当出现错误时可以向后调节参数

<font color="red">softmax</font>
softmax函数：逻辑函数的推广，归一化指数函数。

$$\delta(z)_j = \frac {e^{z_j}}{\sum_{k=1}^K e^{z_k}}$$

> 将任何一个元素都压缩到了指数域，并满足归一化,并且保证了归一化结果在[0,1]之间
> 将原数压缩为指数来做有什么感觉


<font color="red">sigmoid</font>
sigmoid函数，数学表达式
$$f(x) = \frac {1}{1 + e^{-x}}$$
优点：可以把任何值映射到[0,1]之间；导数的计算公式方便$f'(x) = f(x) (1-f(x))$
https://blog.csdn.net/saltriver/article/details/57531963

扩展：[指数分布族](https://blog.csdn.net/saltriver/article/details/55105285)
### 3.1.1 spectral-based

spectral CNN: 直接将filter设置为了参数
ChebySheve: 重新改进了公式，定义了filter的计算，并使得公式的更新的复杂度降低，而且只需要用到空间中的局部信息
1阶：沟通两类, 在batch-learning时会指数级增加开销
AGCN: 扩展了Laplacian矩阵的适用范围，为每个图重定义了一个剩余矩阵，即完成有向图到无向图的转换

> 所以这里的黑盒实际上就是数学表达式计算

原本2D卷积是filter设置的是尺寸，而这里的filter有什么含义呢？
> 就是从信号的角度，过滤得程度吧？？

所以说这里的GCN是有很多参数，然后反馈参数修正是当end时再来修正参数，对吗?

filter为什么是domin dependent, 比如两个不同的domin,人和物品，显然感觉不能吧

### 3.1.2 spatial-based

GNNs,GRU, SSE
MMNN, GraphSage

recurrent GCN全一样
composition GCN全不一样

#### 1) recurrent
GNNs: 每个GCN考虑当前节点的属性，相关边的属性，邻居在上一步的表示，邻居的属性
GRU: 每个GCN将上一步的状态和邻居节点当前的状态放在GNU中进行考虑
SSE： 每个GCN, 随机地选取t个节点，进行更新表示然后随机第选取P个已经更新？的节点来更新梯度

1. GNNs 全是用的一个把所有因素都进行考虑的函数，唯一的点就是要保证这个函数收敛
循环神经网络
2. GRU 门控神经网络： 循环神经网络在时间步数较大或时间步数较小时，循环神经网络的梯度会衰减和爆炸 


<font color="red">RNN</font>
循环神经网络 RNN
神经网络可以看作任意函数的黑盒子，目的是为了解决时序数据中存在的对应关系。
中间因为出现循环，所以某个状态会对应T步，但是还是基本的权重更新公式
> 这里待推导！！！
> 所以这里有个时间t原来是这么回事
> 这里的时间步数就是关联程度的感觉
https://zhuanlan.zhihu.com/p/30844905

https://zybuluo.com/hanbingtao/note/541458


<font color="red">GRU</font>
实际上的感觉就是认为RNN那样直接进行步长来更新（因为该步也要考自己来设置），这样可能导致问题，衰减或爆炸，所以这里的想法就是加入了门控，即在中间可以根据隐藏状态和当前出现的值来决定是否进一步计算还是改变当前值进行另一种计算。
https://zh.d2l.ai/chapter_recurrent-neural-networks/gru.html

3. SSE

随机异步更新节点的表示，考虑历史状态和所有新状态
> 神奇？ 暂未涉猎， 直觉上会收敛

> 这里统一的$h_v^t$是指v在第t个GCN所得到的值，注意这里的t是全局所设置的网络结构

#### 2) Composition Based
MPNN： 每一步的GCN综合上一步的信息和邻居节点的更新操作集合的信息
GraphSage: one batch在线学习的特点，根据域，聚集当前的邻居信息，使一个点成为最终状态；然后，为每个邻居进行更新；最后，知道所有的节点都成为了最终状态

- MPNN

message passing: 信息函数，信息更新函数
这里的信息函数考虑到了上一步的信息，和关于邻居节点的所有更新操作
更新函数考虑到了自己的上一步的表示，邻居节点的表示和边的权重
readout
关于最后的值的读取函数

- GraphSage
看公式的话是每个位置有个权重，并且将上一次的点的状态和邻居节点的状态进行了考虑。

过程？？ 不是很清楚？
一种观点：

这里的节点是怎么选取的？ 
> 它说的是保证各种聚集参数是对的，但是并未说其他的啊？？

首先对一个节点进行更新，然后根据邻居节点产生该节点的状态，然后使用中间节点的最终状态来预测和反馈错误。

也就是说先考虑一个点，然后传递错误到其他点，但是我会进行标记这个点已经不可再取。 而且当新的数据来临时，也可以采用直接训练的想法

#### 3) 杂项
DCNN(缩小图的转移举证)
PATCHY-SAN(先全局排序，CNN)
LGCN(得到邻接节点的特征矩阵处理排序后，CNN+子图搜索)
MMN(建议考虑边的权重，使用高斯核来学习)

- DCNN
模拟压缩图的扩散过程，所以这里除了权重，函数f应该具有缩小尺寸的功能
但是需要存储转移矩阵，耗费内存
- PATCHY-SAN
先将节点排序，然后后面的所有做法就是选取特定的值，然后使用标准的CNN来做
- LGCN * 
node level
在每次做的时候，对特征矩阵进行分析，选取特征矩阵的前多少行来做。
子图训练
- MMN
*考虑了空间位置关系，即便的权重，建议使用标准的高斯核来学习参数调节边的权重 
> 绝大部分采用CNN,也有用GCN的

### 3.1.3 总结
recurrent 节点的稳定状态
composition 可能获得高级别的信息

但是要放隐藏状态，内存

子图训练： GraphSage确定一个的最终状态先,SSE完全随机的思想

复杂的网络结构取得成果
1. 增加深度和宽度
2. 多图维护信息
3. 超参数学习接收域的大小

### 3.1.4 池化
chebNet首先进行池化，使得一开始的结构得到了改进，进一步取得了更好的效果
DGCNN 池化进行预处理排序
DIFFPOOL 使用两个GCN, 一个embed, 一个pool, 用来整合异质的GCN。

基于空间的在效率上可以使用batch和smapling提高效率，在泛化性上可以共享权重;在灵活性上，可以处理多源输入
## 3.2 GAN

- GAT
就是考虑所有邻居节点的表示和考虑邻居节点的权重
> 不同的子空间是指的，随着当前所处的不同位置，可能得到的数据不同，所以就把所有子空间的邻居节点的位置堆叠吗。感觉是这样的
> 这里的子空间也可以看做是某个模型

- GAAN
在聚集不同子空间中的信息时，考虑不同的权重

- GAM
不考虑所有的情况，考虑其中一段一段的状态，因为LSTM非常好用，就用它

- AW
目标是node embedding, 即为了学习转移矩阵。操作的想法是，构造转移矩阵，并且当前的共现矩阵为所有转移矩阵之和的某种表示
> ? 如何体现，应该是转移矩阵的参数体现吧


**总结**

- 注意力为邻居赋予权重
- 整合多个模型
- 指导random walks

GCN每个聚集时，直接按照某种特定的属性进行聚集
GAN: 首先会通过一个神经网络结构来获得权重指派，等于说多加了一层网络结构

## 3.3 GAE
编码：得到最终的编码结果

GAE: GCNs, 评价调节
AGRA: GCN产生，GAN训练
NetRA: 序列到序列，random walks
DNGR: 堆叠矩阵+噪声，基于统计数据
SDNE: 使用指标，评价邻居，评价结果，正则化项
DRNE: 重建节点，LSTM聚集函数

- GAE
GCNs, 然后定义评价函数
- AGRA
GCN产生，GAN评价,
产生时服从某一先验分布，评价时选出服从分布的

- 混合变体
    - NetRA: 通过序列到序列结构的random walks来重建序列
    - DNGR: 用堆叠的降噪的自动编码器来重建矩阵。具体做法是通过统计同时出现的数量与单独出现数量的比值的最大值来决定最终的共现矩阵的值。因为是直接根据数据特性来统计，所以所学得的东西应该是非线性的。另外一点就是，在做的时候随机增加了噪声
    - SDNE: 使用了两个指标，一个是隐藏节点表示与邻居节点隐藏表示之间的距离；一个是节点的输入与重新构造之间的距离（这里考虑了惩罚项）；最后，优化目标即为三个项+正则化项
    - DRNE: 直接考虑重建节点，设置损失函数，定义为所有邻居节点表示值与聚集函数(LSTM)的差

DNGR+SDNE：仅考虑拓扑结构
邻接矩阵的稀疏性：
GAE: 重新赋予权重； NetRA线性化；DNGR密集矩阵; SDNE: 惩罚

A+X经过Encoder, 再经过GCN, 得到一种简单的表示，然后经过非线性激活函数？ 就可以由decoder重构出邻接举证吗？
> 这前后是不是应该有严格的数学来做？加一层GCN，训练得到的参数是否还满足性质

## 3.4 GGN
重新生成图

- MolGAN
分布->Generator->密集邻接矩阵tensor(张量)->sample产生Graph-> GCN-> Disciminator产生反馈和进行调节
> 需要预知分布，共同产生边和节点

- DGMG
采用加点和边的方式，有一个停止的标准；当decision为假时，加边；为真时，评估将加入的节点连接到已存在的节点的可能分布，然后随机选择一个点。当一个新的节点和它的连接都被加入图中，更新图的表示。
> 需要预知分布，采用序列化的方式来做
> 图的表示用来计算前面的decision, 所以该方法又称为根据图的表示产生图的节点和边

- 杂项
    - GraphRNN
    两个RNN，对于graph和level，操作在二进制序列上。如何添加根据假设分布来做。对于graph为了训练，用BFS线性化为序列
    > 采用序列化的方式来做，利用了统计数据？
    - NetGAN
    产生器LSTM产生合理的random walks, 测试器GAN挑选，最后获得共现矩阵
    > 利用统计数据？ 同时产生节点和边

序列化的方式在于，长序列会带来问题？？
其他方式维护全局属性难
最近的采用变化的自动编码器做，增加输出空间
没有方法有很好的大规模可扩展性


产生+训练

## 3.5 GSN




DCRNN能处理长时间
CNN-GCN有效取决于1DCNN
ST-CNN考虑瞬时流作为节点的边，导致邻接矩阵平方增长，会导致计算图卷积层的开销增大，但是要堆叠很多次
Strutural-RNN需要先验知识来分割语义组

空间+瞬时

# 4. 其他

## 4.1 建议
go deep
感受野
扩展性： 什么时候具有扩展性？？ 快采样（在线算法）和子图训练。不够
动力学和异质性：图的结构固定，节点和边不一定来源相同
## 4.2 batch learning vs stochastic learning

batch learning: 批量学习，是一种离线的学习方法
> 离线学习，输入数据时不处理数据；训练数据时才处理数据
> 总是全局最优的，但是耗时

stochastic learning: 随机学习，在线学习
> 每输入一种数据都进行修正，这里默认的单位为1，即有一个样本时就进行修正
> 那么问题的关键就在于这个样本是否有问题，即该模型就容易搜到噪声的影响，有问题的数据称为噪声； 但也有好处，能够节省时间。
> 易陷入局部最优

深度网络的实现中都采用了两种方法的结合，即每次训练使用10个或者别的合适数目的样本，即可以稍微消除噪声的影响，又可以进行在线学习

https://blog.csdn.net/Buyi_Shizi/article/details/51454549

## 4.3 无结构化数据 vs 结构化数据

结构化数据即数据能够用二维表结构来逻辑表达实现的数据，通常每行和每列都有具体的含义

无结构数据即不能用数据二维逻辑来表现的数据，比如所有格式的办公文档、文本、图片、HTML等等

无结构化数据更难被计算机理解

https://zhuanlan.zhihu.com/p/29856645

## 4.4 dropout
为了解决过拟合现象，也就是用小数据集训练过于复杂的网络
训练深度神经网络，总会遇到两大问题：过拟合+费时，一定程度可以达到正则化的效果。

dropout就是在训练深度神经网络中，每次训练中，忽略一半的特征检测器（让一半的隐层节点值为0），这样可以明显地减少过拟合现象。

简单地说就是让某个神经元以一定的概率停止工作
https://zhuanlan.zhihu.com/p/38200980

## 4.5 cross entropy loss

回归问题常用的损失函数是均方误差MSE
分类问题常用的损失函数是交叉熵
> 描述了两个概率分布之间的距离
> 预测函数得出的结果应该是分为每一类的概率

在得到概率之前，softmax函数经常在交叉熵前面，用于把结果处理为[0,1]之间的概率

首先，如果用实际分类错误率来做，太粗略
为什么不用MSE？因为softmax + MSE会产生非凸函数，不具有好的性质。
> 这里自己可以进行分析

所以最终用的是交叉熵，softmax+交叉熵会产生凸函数，结果具有凸函数的性质。
> 损失函数也会参与反馈传递误差的过程吗？

$$H_y(y) = - \sum_i y'_i log(y_i)$$
为什么回归问题不用交叉熵?
因为`log-1.5`没有意义。
> MSE是在欧式距离为误差度量的情况下，由系数矩阵所张成的向量空间内对于观测向量的最佳逼近点


结论：
traning中需要用最反映实际情况的，即分类问题用交叉熵
> 交叉熵用在training这里是体现训练误差的，损失函数的设置是用来指导如何进行梯度下降的。

validation/testing中，关注的就是分类的错误率，所以用分类错误率即可。

https://blog.csdn.net/xg123321123/article/details/80781611


https://www.cnblogs.com/pinard/p/5970503.html

# 5. 简要

## 5.1. GCNS
### 5.1.1 spectral-based

spectral CNN: 直接将filter设置为了参数
ChebySheve: 重新改进了公式，定义了filter的计算，并使得公式的更新的复杂度降低，而且只需要用到空间中的局部信息
1阶：沟通两类, 在batch-learning时会指数级增加开销
AGCN: 扩展了Laplacian矩阵的适用范围，为每个图重定义了一个剩余矩阵，即完成有向图到无向图的转换

### 5.1.2 spatial-based
- recurrent
GNNs: 每个GCN考虑当前节点的属性，相关边的属性，邻居在上一步的表示，邻居的属性
GRU: 每个GCN将上一步的状态和邻居节点当前的状态放在GNU中进行考虑
SSE： 每个GCN, 随机地选取t个节点，进行更新表示然后随机地选取P个已经更新？的节点来更新梯度

- Composition Based
MPNN： 每一步的GCN综合上一步的信息和邻居节点的更新操作集合的信息
GraphSage: one batch在线学习的特点，根据域，聚集当前的邻居信息，使一个点成为最终状态；然后，为每个邻居进行更新；最后，知道所有的节点都成为了最终状态

- 杂项
DCNN(缩小图的转移举证)
PATCHY-SAN(先全局排序，CNN)
LGCN(得到邻接节点的特征矩阵处理排序后，CNN+子图搜索)
MMN(建议考虑边的权重，使用高斯核来学习)

### 5.1.3 池化
chebNet首先进行池化，使得一开始的结构得到了改进，进一步取得了更好的效果
DGCNN 池化进行预处理排序
DIFFPOOL 使用两个GCN, 一个embed, 一个pool, 用来整合异质的GCN。

## 5.2. GAN

GAT: 考虑邻居及诶点的表示和邻居节点的权重。并且考虑子空间，不过每个子空间都是相同的权重
GAAN: 不同的权重
GAM: 考虑用的因素不再全部，而是一段一段
AM: 目标是node embedding, 直接学习转移矩阵，由注意力指导random walks

## 5.3. GAE
GAE: GCNs, 评价调节
AGRA: GCN产生，GAN训练
NetRA: 序列到序列，random walks
DNGR: 堆叠矩阵+噪声，基于统计数据, 统计出来的结果
SDNE: 使用指标，评价邻居，评价结果，正则化项
DRNE: 重建节点，LSTM聚集函数

## 5.4. GGN
MolGAN: 基于分布 ，产生图和点，然后产生Graph,GCN,再调节
DGMG: 获得一个图的表示后，做一个决定，根据决定来加边，或从分布中加点,操作在序列上
GraphRNN: 两个RNN, 一个graph训练，一个edge由分布来做
NetGAN: LSTM产生合理的random walks, 测试器筛选，最终获得共现矩阵

## 5.5. GSN
DCRNN: 出度入度-空， DCGRU-瞬时
CNN-GNN: GCN空间，1DCNN-瞬时
ST-CNN: 瞬时流作为边，同时考虑。用邻接举证距离的和作为最终接地那的值
Strutual-RNN: 语义组，GCN瞬时；nodeRNN接edgeRNN输出空间


