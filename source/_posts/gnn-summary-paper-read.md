---
title: gnn summary paper read
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:52:13
tags:
- gnn
- paper
categories:
- [gnn, summary]
---

# 1. Abstract
1. 深度学习应用很广，特别是在图像分类和视频过程识别以及自然语言理解
2. 这些任务的数据都表示在欧式空间中
3. 有其他对于非欧式空间的需求， 将图表示为复杂的关系和交互依赖
4. 图的复杂性很大

文章：
1. 一个调查报告，深度学习在数据挖掘和机器学习领域的运用，
2. 将图的状态设计神经网络的计数分为不同的种类
3. 图的建设性的可替代结构, 包括图注意网络，图自动编码器，图生成网络和图的空间瞬时网络结构
4. 接下来，讨论了图的神经网络在各个领域的应用和找了开源代码和benchmark算法针对不同的任务。
5. 最后，提出了有意思的方向


# 2. Introduction

CNN，LSTM,等的成功

!欧式距离 vs 非欧式距离

？ 核心机器学习算法样例之间一般互相独立，但是图数据中节点与节点之间有着相互的关系，比如引用，友元，相互作用

如何降低图的复杂性，最基本的方法还是模拟卷积的思想，即卷积核的大小可以来模拟图的不同权重值

> 但是不与图像数据相同的是，图中每个节点的邻居节点是无序的而且大小不确定

<font id="fig1" color="red">fig1</font>

![](fig1.png)

> ? 这里说的是进行取平均？但是实际是怎么做的啊？

## 2.1 GNN过去发展的历史

第一个就是基于卷积的简单表示[21]
基于光谱图理论
[22],[23],[24] 
对图的全局做处理，无法很好地并行化和应对大规模图

> 这些方法的思想都是直接通过节点的邻居信息的特点， 加采样来做的

## 2.2 近期的工作
其他人做的不好

## 2.3 graph neural network vs network embedding
graph neural network
grph embedding

network embedding
> 目标是将网络节点表示在一个低维空间，并且保留拓扑和本身的内容信息
> 用途是分类，聚类和推荐系统(支持向量机)
> 通常是半监督算法，常见类别有
> - matrix factorization
> - random walk
> - deep learning

![](fig2.png)

## 2.3 贡献
1. 新的分类学，五类
graph convolution networks GCN
graph attention networks GAN
graph auto-encoders GAE
graph generative networks GGN
graph spatial-temporal networks GSN

2. 易于理解
3. 丰富的资源
4. 未来的方向

# 3. Definition
![](table1.png)

# 4. Categorization and frameworks
## 4.1 种类
![](works.png)

### 4.1.1 GCN 
> 卷积+池化

1. basic
通过考虑自己和邻居的特征信息，从而学习到表示该节点的函数

![](GCN1.png)

2. +pooling
<font id="GCN" color="red">GCN</font>

![](GCN2.png)

> 卷积的理解：卷积核主要的目的是利用卷积核的设计使得能够保留原图像的局部信息；并且实现原图像的；卷积实际是一个抽象函数
[常见的卷积方法](https://zhuanlan.zhihu.com/p/54033473)

> 池化的理解：当对每个图像做完卷积后，如果直接将特征拿去学习会太消耗；所以会考虑使用池化的方法，即学习这些数据中的聚集特征，比如最值、平均值、众数等等
[常见的池化方法](https://blog.csdn.net/chenyuping333/article/details/82531047)
### 4.1.2 GAN
> Attention, 加入了注意力机制，思想即关注重要信息；主要的做法是为更重要的节点，路径或者模型安排权重

> 目标与GAN相同，试图学习一种新的结点的表达方式，能够整合邻居节点，随机路径和候选模型

1. Attention vs Convolution
 
![](GAN.png)
> ???

### 4.1.3 GAE
一种半监督学习的框架
目标在于通过编码器学习到低维表示，并能够通过译码器翻译回去

graph embedding

![](GAE.png)

### 4.1.4 GGN
目标在于从数据中学习到貌似合理的结构

通过一个图的经验分布来产生模型，本身就极具挑战的一件事

> 一般的做法是首先形成产生节点和边，是否可替代，然后应用产生对抗训练进行筛选

应用在化学成分分析中比较多

### 4.1.5 GSN
目标在于学习未见的模式通过暂时性空间结构的图
应用比如在交通预测和人类活动预测，图的结构往往是变化的
主要的想法就是同时考虑空间依赖和瞬时依赖
> 空间依赖——GCNs来做
> 瞬时依赖——RNN或CNN来做

![](GSN.png)

## 4.2 框架

GCNs试图去复试CNN的成功在图中运用spectral theory和spatial locality信息

- 输入
图的结构信息和节点的内容信息 
- 输出
不同的图的分析任务
    - Node-level
    关于节点的回归或者分类任务，只需要直接给出节点的表示，通常用感知机或者softmax函数作为GCN的最后一层
    - Edge-level
    关于边的分类或者链接预测任务。为了预测关于边的lable/connection的强度，一个额外的函数需要能够将两个节点的表示作为输入
    - Graph-level
    关于图的分类任务，为了获得在图级别的联合表示，pooling需要能够把一个图改为子图来求和或者平均节点的表示

**main GCNs methods**

![](summary.png)

**end-to-end Traning  Frameworks**

> 这里的end-to-end理解是关注输入到输出吧
这里根据训练任务和标记信息是否可以直接获得可以分为监督、半监督和无监督学习

 - 半监督学习 node-level classification. 
 首先通过部分有标记的图的节点的网络学习到robust model, 它可以高效地为未标记数据进行分类。
 end-to-end框架可以通过堆大量的跟随softmax层用来做多标记学习
 > 对！这里会给出一个网络结构只有部分节点有标记
 - 监督学习 graph-level classification
 给出整个图数据集，图级别的分类任务旨在预测关于整个图的标记
 end-to-end框架可以通过GCNs加polling来完成, 最后加些softmax层, [GCN结构](#GCN)
 - 无监督学习 graph embedding
 这些算法采取两种角度来开发edge-level的信息
    - 编码器+译码器
    - 利用负采样的方法，采样一部分节点作为负pairs，并且已经存在一些节点在图中是正样本了，最后应用一个逻辑回归层
    > 负采样？？？

# 5. Graph Convolution Networks
spectral-based
> 介绍filters从图信号的角度，将卷积的操作看作消除噪声

spatial-based
> 从邻居节点收集特征信息

当GCNS操作在node level, pooling可以交替在GCN layer之间，从而将图转换为高级别的子结构

## 5.1 基于谱分析

### 5.1.1 基本套路（背景）

然后每个节点利用的是拉普拉斯矩阵，以及特征值分解来做
首先，定义
$$L=I_n - D^{-1/2}AD^{-1/2}$$
robust 数学表示

实际上
$$L = \frac{A_{21}+A_{31}+..+A_{n1}}{A_{11}+A_{21}+A_{31}+..+A_{n1}}$$
然后，该矩阵一定为半正定矩阵，所以得到特征向量U和特征值

那么，由可逆运算就可知
$x=U (U^Tx)$
然后如果再对x加上一个filter，那么就有
$x*_G g_\theta = U(U^T(x) .* U^T(g))$
> 这里.*是一种定义的新的运算，就是矩阵的每个元素进行相乘

化简后结果即为
$$x*_G g_\theta = U g_\theta U^T(x)$$ ？
> 这里实际上就形成了图的卷积操作，这里的关键就在于如何进行设计$g_\theta$

### 5.1.2 实际的方法
1. Spectral CNN
![](equation4.png)
假设$g_\theta = \Theta_{i,j}^k$是可学习的参数
这里假设了转换前和转换后参数的个数，即又称为channel

2. Chebysheve Spectral CNN
$g_\theta = \sum_{i=0}^K \theta_i T_k(\widehat{\Lambda})$
这里$T_k是递归函数定义$
经过化简操作，可以发现？？
$$x*_G g_\theta = \sum_{i=0}^{K-1} \theta_i T_i(\widehat{L})x$$
这里$\widehat{L} = 2L/ \lambda_{max} - I_N$
> 将时间复杂度从O(N^3)降低到了O(KM)
而且$T_i(\widehat{L})$计算时，只需要空间中的局部信息。

3. 1阶ChebNet  K=1, $\lambda_{max}=2$
该方法沟通了spectial-based和spatial-method。
在batch training中，随着层数的数量的增加，计算开销会指数级扩张？？？

4. Adaptive Graph Convolution Network
为了扩展Laplacian矩阵的应用范围，因为Laplacian矩阵只能建模在无向图上。定义了一个叫做剩余矩阵的东西，可以用用来衡量两个节点之间的距离
尽管可能计算多余的东西，但是时间复杂度只有$O(N^2)$

### 5.1.3 评价

Spectral CNN依赖于拉普拉斯矩阵的特征值分解
1. 图的任何一点扰乱将导致特征分解的重做
2. 学习到的filter是domin dependent???, 意味着他们不能应用再一个不一样的结构中
3. 特征分解需要O(N^3)的时间和O(N^2)的空间

ChebNet and 1st ChebNet
学习到的filters定义在局部的空间，所以可以在图的不同位置进行共享。

**总结**
关于该方法需要将所有图加载到内存中进行graph convolution, 不利于解决大的图???ChebNet

## 5.2 Spatial-based

[Fig1](#fig1)
figA, 对于image而言，每个像素点直接与周围的像素点像关联
figB, 为了扩展节点能容纳的深度和宽度，一个常用的方法就是将图的多个卷积层叠加在一起

Recurrent-based: same graph convolution layer来更新隐藏的表示

Composition-based: differnt graph convolution layer

比较

![](rb-cb.png)

### 5.2.1 Recurrent-based
- impose constraints on recurrent functions
- employ gate recurent unit architectures
- update node latent representations asynchronously and stochastically

#### 1) Graph Neural Networks

递归计算直到收敛，扩散直到相等

$$h_v^t = f(l_v,, l_{co}[v], h_{ne}^{t-1}[v], l_{ne}[v])$$

$l_v$-node v的属性
$l_{co}[v]$-node v相关的边的属性
$h_{ne}^{t-1}[v]$-v's 邻居在第t步的表示
$l_{ne}[v]$- v's邻居的属性

为了保障收敛，f函数应该是收缩映射的
f是神经网络，那么对于参数的雅克比矩阵？应该得到惩罚

the Almeida-Pineda algorithm
#### 2) Gate Graph Neural Networks
gated recurrent units——循环函数
recurrence循环

$$h_v^t = GRU(h_v^{t-1}, \sum_{u \in N(v)} W h_u^t)$$

与GNN不同的是，GGCN通过时间上的后向传播来传递参数。

好处是不用保证收敛性，缺点会牺牲时间和空间上的效率，需要多次在全部节点上计算recureent函数，以及需要将所有状态都存储在内存中

#### 3) Stochastic Steady-state Embedding

SSE： 每个GCN, 随机地选取t个节点，进行更新表示然后随机第选取P个已经更新？的节点来更新梯度

为了提高预测效率，SSE随机异步地更新节点的表示。为了保证收敛性，recurrent函数定义为了历史状态和新状态的平均权重

$$h_v^t = (1-\alpha ) h_v^{t-1} + \alpha  W_1 \delta (W_2[x_v, \sum_{u \in N(v)} [h_u^{t-1}, x_u]])$$

![](SSE.png)

### 5.2.2 Composition Based

构成

#### 1) Message Passing Neural Networks

产生已经存在的图卷积神经网络的一个框架。

包括两部分
- message passing phase

通常运行T步的基于空间的图神经网络算法。
图神经网络定义了一个信息函数和一个更新函数

$$h_v^t = U_t (h_v^{t-1}, \sum_{w \in N(v)} M_t(h_v^{t-1}, h_w^{t-1},e_{vw}))$$

> $M_t$信息函数， $U_t$更新函数

- readout phase

通常对应于池化操作，产生基于整个图的一个表示

$$\widehat{y} = R(h_v^T | v \in G)$$

$\widehat{y}$用来做最终的graph-level的任务

> 需要确定不同形式的Ut()和Mt()

#### 2) GraphSage*

使用聚合的概念定义图的卷积

> ?必须保证对节点的聚合值的排序的排列，那么其他的需要保证吗？
使用mean, sum, max等函数

$$h_v^t = \delta(W_t \cdot aggregate_t (h_v^{t-1}, \{h_u^{t-1}, \forall u \in N(v)\})$$

这里不是更新所有节点，而是进行batch-traning,适合扩展，说明是可以扩展到很大的数据集的？

3 steps
1. 对一个节点的k-hop邻居节点进行固定大小的采样 ？
> 这里的采样范围是什么？ 一个圆吧
2. 通过聚集邻居节点的所有信息，最终产生该中心节点的最后状态
3. 使用中间节点的最终状态来做预测和前馈错误

![](GraphSage.png)

假定$t^{th}$hop的邻居数量是$s_t$, one batch的时间复杂度为 $O(\prod _{t=1}^T s_t)$, 指数级别
> 作者发现t=2是GraphSage已经取得很好的成绩

### 5.2.3 Miscellaneous Variants of Spatial GCNs

#### 1) DCNN 扩散卷积神经网络
> encapsulates 压缩

通过图卷积网络压缩图的扩散过程

一个隐藏的节点的表示通过将输入 ？ 得到状态转移矩阵

维度搞

$$Z_{i,j,:}^m = f(W_{j,:} \odot P_{i,j,:}^m X_{i,:}^m)$$

P是概率，X是特征， W是权重
> 这里H是什么？ 应该是表示值什么的吧？
尽管转移矩阵的高维中覆盖了大量的域，需要$O(N_m^2H)$的内存，会造成巨大的问题
#### 2) PATCHY-SAN

用一个标准的卷积神经网络CNN来解决图分类问题 graph classification tasks

将图结构数据转换拿为网格结构数据

1. 给每个节点安排一个排序，基于的标准有多种多样，比如度，形成图的标记
2. 为每个节点根据图的标记选择固定数量的邻居节点
3. 使用标准的CNN来进行学习

CNN的优点：能够基于排序保证移动不变性

#### 3) LGCN

建议基于节点特征的信息进行排序

vs PATCHY-SAN: 

产生 node-level输出，对于每个节点聚集了关于邻居节点的特征矩阵，然后再特征矩阵中为每列进行排序， 将头k行的排序特征作为输入

在最后，采用1D CNN在相关的输入上得到隐藏节点的表示

> PATCHY-SAN需要没复杂的预处理，而LGCN不需要预处理。 
> LGCN建议了子图训练的策略，即使用一小部分样本的子图进行min-batch

#### 4) Mixture Model Network

试图利用标准的CNN来统一非欧式空间域
> 大部分基于空间的方法在考虑的时候都忽略了节点与邻居将的相对位置关系

做的方法就是为每个邻居节点的关系进行定义，然后赋值不同的权重。

> 绝大部分采用CNN,也有用GCN的
MoNet建议了使用高斯核来进行学习参数来动态调节权重

### 5.2.4 Summary

循环神经网络获得节点的稳定状态
基于组成的方法试图集合高级别有用的信息

两种结构中，每一层在训练中都需要更新所有节点隐藏的状态，但是内存往往很难将这些状态存下。

所以，进行子图训练的方法：
1. GraphSage
2. 随机异步训练的方法 SSE

但近几年的进步还是通过空间方法来建立更复杂的网络机构
1. 用门控机制来控制节点的深度和宽度 GeniePath
2. 设计两个图的卷积网络来切入本地的一致性和全局一致性 DuraGCN
3. 超参数来影响节点接收域的大小

## 5.3 图池化模块

很重要， CNNs, down-sampling

其中，mean/max/sum pooling是最原始和最有效的加快计算，降低维度的方法

$$h_G = mean/max/sum(h_1^T, h_2^T, \cdots, h_n^T)$$

已经证明，先池化可以减少傅里叶开销

ChebNet  对输入图形首先进行coarsen处理，如图5a; 其次，顶点的输入和coarsen后的版本在平衡二叉树中得到了改进。
当节点最粗糙的版本传递到了平衡二叉树的最低级别时产生了一个特别好的信号，池化带来巨大好的效果

DGCNN SortPooling, 先按SortPooling得到一个顺序，再依次取出这些点染成WL colors, 再排序。根据排序的结果取最有用的前k个，如果不够则置为默认值(0).

这个方法增加了池化网络来解决一个图结构的任务，该任务需要保证排列不变性

DIFFPOOL

产生图分层的表示，然后不止于CNNs相结合，并且考虑使用其他变化的神经网络结构。
> 池化层嘛，所以难道这里说的是这里的池化层可以处理不同CNNs来的东西
与之前的方法相比，不再简单地聚聚一个图中的节点，而是提供一种大类的解决办法来分等级低池化不同的输入图
> 这里的意思是该框架可以对不同的图进行操作吗？

主要的想法是学习一个cluster S在不同的层次l代表不同的内容，如$S^{(l)} \in R^{n_l \times n_l + 1}$.两个分割的GNNs同时处理两个输入cluster的节点特征$X^{(l)}$ 以及coarsened的邻接矩阵$A^{(l)}$用来产生新的分配矩阵$S^{(l)}$以及embedding矩阵$Z^{(l)}$
$$Z^{(l)} = GNN_{l,embed}(A^{(l)}, X^{(l)})$$

$$S^{(l)} = softmax(GNN_{l,pool}(A^{(l)}, X^{(l)}))$$

> ？？？ cluster在这里是指啥意思？？，聚集的意思， 等于是分层，l是层次，所以说这里l是input的第l个元素？，然后A,X是每个元素的特征？

可扩展到多种标准的GNN模块，他们拥有相同的数据输入，却有种不同的参数，因为各自的角色不同。
$GNN_{l,embed}$可以产生新的embeddings 而$GNN_{l.pool}$产生输入节点可能安排到哪个clusters的一个指派。
结果，$S^{(l)}$的每一行代表一个$n_l$节点或者clusters在layer l, 每一列表示下一层的$n_l$.
一旦我们拥有了$Z^{(l)}$和$S^{(l)}$
$$X^{(l+1)} = S^{(l)^T} Z^{(l)} \in R^{n_{l+1} \times d}$$
> 通过S的支配去Z的新的embdding中计算新的X的表示, 初始化的Z应该为节点的表示
$$A^{l+1} = S^{(l)^T} A^{(l)} S^{(l)} \in R^{n_{l+1} \times n_{l+1}}$$
> 使用邻接矩阵为输入，然后产生一个变粗的coarsened的邻接矩阵，根据clusters中每个pair的链接性的强弱
总之，DIFFPOOL重定义了图的pooling模块，通过使用两个GNNs来聚集节点。 任何一个标准的GCN模块都可以进行联合，不仅可以使性能增强，还可以加快卷积的速度

> 两个GNNs, 一个来获得新的表示，一个来获得新的指派；如果是多个，是用来构成一个大矩阵的吗？

## 5.4 对比 Spectral and Spatial
在早期，主要的进步是靠基于频域的，但是有着明显的缺点。
- efficiency:  图一改变就需要重新特征分解， 基于空间的可以只训练a batch of nodes, 当节点增加时，可以使用sampling技术来提高效率
- generality: 基于频域的假定一个固定的图，使他们普遍变为一个新的不同的图。基于空间 的图，他们在每个节点上做卷积，他们的权重依然被不同的位置和结构共享
- flexibility: 基于频域的只适用于无向图，Laplacian矩阵并未在有向图上有很好的定义，唯一的方法只有将有向图转为无向图。基于空间的更加灵活，可以处理多源的输入，比如边的特征和方向，因为这些输入可以聚集在聚集函数中
所以， 基于空间的模型更受欢迎

# 6. Beyond Graph Convolutional Networks

## 6.1 GAN
注意力机制在序列基准的任务中已经成为了标准，优点在于能够集中在最重要的东西。 这一特性被证明是非常重要的。

用注意力带来很多很出，在聚集的时候
### 6.1.1 方法

#### 1) GAT


就是考虑所有邻居节点的表示和考虑邻居节点的权重
> 不同的子空间是指的，随着当前所处的不同位置，可能得到的数据不同，所以就把所有子空间的邻居节点的位置堆叠吗。感觉是这样的

$$h_i^t = \delta(\sum_{j \in N_i} \alpha (h_i^{t-1},h_j^{t-1}) W^{t-1}h_j^{t-1})$$

$\alpha(\cdot)$是注意力函数，控制节点和邻居节点之间的贡献
为了在不同的子空间中学习权重

$$h_i^t = ||_{k=1}^K \delta(\sum_{j \in N_i} \alpha_k (h_i^{t-1},h_j^{t-1}) W_k^{t-1}h_j^{t-1})$$

这里||表示级联
> ?

#### 2) GAAN
考虑multi-head, 不过不是分配等选中，屙屎进行不同的权重分配

![](equation22.png)
#### 3) GAM
循环神经网络来解决图分类问题，通过访问不同序列的重要节点来展示图的各部分的信息
![](equation23.png)
$f_h(\cdot)$是LSTM网络
$f_s$是网络在过程中选取当前节点的邻居，并且优先选取有高排名的节点，它有policy network生成

$$r_t = f_r(h_t;\theta_r)$$
其中$r_t$是随机排名向量预测哪个节点更加重要，从而应该给该节点配置更高的优先级
$h_t$包含了历史信息，agent已经从图中获得的。agent是用来给图的标记做预测的
#### 4) Attention Walks
不像DeepWalk使用固定的先验，Attention Walks产生共现矩阵通过不同的注意力权重

$$E[D] = \tilde{P}^{(0)} \sum_{k=1}^C a_k (P)^k$$
这里D是共现矩阵
$P^{(0)}$是初始化矩阵
P指示转移矩阵的概率

### 6.1.2 总结
注意力机制对图神经网络有3种方式
1. 在整个邻居节点时为每个邻居赋予不同的权重
2. 通过注意力权重整合多个模型
3. 通过注意力来指导random walks

它们同时也可以认为是基于空间的卷积神经网络
GAT和GAAN的优点在于可以动态学习邻居节点的权重
但是会带来时间和空间的消耗，当每对邻居被计算的时候

![](GAN.png)

## 6.2 GAE

GCN, GCN+GAN, LSTM+GAN来设计图的自动编码

### 6.2.1 GCN based

#### 1) GAE
$$Z=GCN(X,A)$$
decoder: $\tilde{A} = \delta (Z Z^T)$

优化目标为最小variational lower bound
> 简单理解为之间的距离
![](equation28.png)

#### 2) AGRA

GCN做编码器

GANs 采用min-max策略在产生和测试之间训练生成模型。
一个生成器同概率生成真假模型
然后测试器在真的样例中区分出假的样例

GANs使得节点的表示服从某个先验分布

生成器生成假的模型，测试其判断得出真的模型

### 6.2.2 混合变体
1. NetRA

与ARGA想法相同，不过它还正则化节点的隐藏表示，使其服从先验分布，在对抗性训练的同时。
不是重建邻接矩阵，它是恢复节点的序列，通过sequence-to-sequence结构的random walks中采样得到

2. DNGR
用堆叠的降噪的自动编码器来重建pointwise的互信息矩阵PRMI
当图形采用random walks进行序列化得到的序列是 PRMI捕捉到节点的共现矩阵
![](equation29.png)

其中$|D|=\sum_{v_1,v_2} count(v_1,v_2)$

堆叠的降噪的自动编码器能够学习到高度非线性的规律，不同于传统的神经自动编码器，它增加了输入的噪声，通过随机地将输入入口置为0
当缺失值存在时，学习的潜力也就越大

3. SDNE
保护一阶接近度和二阶联合

一阶proximity定义为节点隐藏表示和邻居节点隐藏表示之间的距离， 目标是尽可能让表示互相接近，特别地，loss函数可以定义为
$$L_{1st}=\sum_{i,j=1}^n A_{i,j} ||h_i^{(k)} - h_j^{(k)}||^2$$
第二节proximity定义为节点的输入和重新构造之间的距离。输入是邻接矩阵中对应的行，目标是用来保护邻居节点的信息，损失函数定义为
$$L_{2nd}=\sum_{i=1}^n ||(\hat{x}_i-x_i)\odot b_i||^2$$
$b_i$是用来惩罚非0元素的输入的，当矩阵高度稀疏时
当$A_{i,j}=0$, $b_{i,j}=1$;$A_{i,j}>0$, $b_{i,j}>1$
因此，总的目标函数就定义为
$$L = L_{2nd} + \alpha L_{1st} + \lambda L_{reg}$$

4. DRNE
重建的目标是节点表示，而不是整个图的数据

loss function
$$L = \sum_{u \in V} ||h_v - aggregate(h_u \in N(v)||^2$$

LSTM作为聚集函数来聚集根据阶排完的节点的序列

### 6.2.3 总结

DNGR和SDNE仅仅使用了图的拓扑数据， GAE,ARGA,NetRA,DRNE同时考虑了两方面

最大的挑战是关于邻接矩阵的稀疏性，导致译码器的正实体的数量远远小于负实体，为了解决这个办法，DNGR构造了密集矩阵，SDNE为0实体处理了惩罚，GAE重新赋值了权重，NetRA将图线性化为序列

## 6.3 GGN
目标是通过观察一系列图产生图。 大部分的方法都是domin specific.
化学分子图SMILES,
一些工作提供可替代性的点或边，然后进行对抗性训练
GCN building blocks或用其他不同的结构

### 6.3.1 GCN based
1. MolGAN
整合了GCN,提高的GAN和RL来产生图。 GAN包括产生器和测试器，与其他一起竞争从而提高产生器的真实性。
产生器试图产生真实的数据，测试器试图辨别。然后还会再增加一个奖惩网络

![](MolGAN.png)

2. DGMG
spatial-based graph convolution networks来获得图的表示。
产生图的节点和边有产生的图的表示来决定


加入一个顶点到增长的图，如此递归操作, 直到达到停止的标准
当decision变为false时，开始加边
当decision为true, 它会评估将性加入的节点连接到已存在的节点的可能性分布，并且由分布随机选择一个节点，
当一个新的节点和它的连接都被加入到图中后，DCMG再次更新图的表示

### 6.3.2 杂项
1. GraphRNN
扩展深度图产生模型通过两个级别的循环神经网络。
graph-level RNN增加一个节点到一个节点序列
edge--level RNN产生一个二进制序列只是新节点和之前产生节点之间的链接
为了训练graph level RNN 为了将图线性化为序列，GrapRNN采用BFS.
为了建模edge-level RNN, GraphRNN假设多元伯努利分布或条件伯努利分布

2. NetGAN
联合LSTM和Wassertein GAN通过random-walk-based方法来产生图
该框架包括两个模块，产生器和测试器
产生器尽可能去通过LSTM来产生最合理的random walks, 测试器尽可能地辨别其中的真假
经过训练，就会获得节点的共现矩阵

### 6.3.3 总结
大问题，不像同步的图像和音频，可以直接由专家判断判断，质量无法评估。
MolGAN和DCNG用来很多额外的知识来评估产生的图的合理性。
GrapRHH和NetGAN通过图的统计数据，比如图的阶数来做
DCNG和GrapRNN产生节点和边序列化的方式
MolGAN和NetGAN共同产生节点和边

之前的方法的问题在于，随着图变大，产生一个长的序列必然带来问题。
后面的方法是一个图的全局属性很难维护
最近的方法采用了变化的自动编码器来产生图，建议了使用惩罚机制的邻接矩阵，然而会增加输出空间从n到n^2
还没有方法具有很好的大规模可扩展性

## 6.4 GSN
需要同时维护空间和瞬时性因素。
这类图往往有一个全局的图结构，但是会随着时间而改变
交通中，每个传感器作为一个节点记录交通的速度，边作为节点之间的距离，
图的任务就是预测节点将来的值或者标记，以及图的标记

GCNs, GCN+RNN/CNN, 循环结构

### 6.4.1 GCN based
1. DCRNN
扩散的卷积——空间因素
GRU序列到序列的结构——瞬时

- 空间
![](equation34.png)
$D_O$是出度矩阵，$D_I$是入度矩阵
为了允许多输入输出channel
![](equation35.png)

P是输入的channel的数量
- 瞬时
循环单元会同时接收上一步临时节点的历史信息和邻居节点的卷积信息。
GRU->DCGRU
![](equation36.png)
为了适应多步的需求，采用sequence-to-sequence的结构

2. CNN-GCN
1D-CNN和GCN学习spatial-temporal 图数据
对于一个输入tensor $X \in R^{T \times N \times D}$, 1D-CNN随着时间轴来收集每个节点的瞬时性信息$X_{[:,i,:]}$
GCN操作来聚集空间信息，在每次的时间步骤中， $X_{[i,:,:]}$ 收集空间信息，输出层是一个线性信息，产生关于每个节点的预测

3. ST-GCN
扩展不同的时间流来作为图的边用一个统一的GCN模型来统计两种信息
它还有一个标记函数来为每个边安排标记，根据两个相关节点之间的距离
所以，邻接矩阵可以用一个K邻接矩阵的求和来表示，K是标记的数量
它应用GCN到不同的权重到K个邻接矩阵上，并且求和他们
![](equation37.png)
### 6.4.2 杂项
- Structural-RNN
目标是预测节点在每个时间节点的标记
它包括两种类型的RNN，nodeRNN和edgeRNN
每个节点和边的瞬时信息由nodeRNN和edgeRNN传递
因为不同的RNN对不同的节点和边会急剧地增加模型的复杂性，所以他们代替为将nodes和edges活粉到不同的语义组。
比如，一个人主题关系图，包括多种两组节点，人节点和物品节点；三种边，人与人的边，人与物品的边，人与物品的边。
在同一语义组的节点或边使用相同的RNN模型
为了合并空间信息，nodeRNN将会将edgeRNN的输出作为输入

### 6.4.3 总结

DCRNN的优点在于能够处理长时间，因为它的循环神经网络结构
CNN-GCN会更优效率得益于1D CNN
ST-GCN考虑瞬时流作为节点的边，将会导致邻接矩阵平方式地增长。一方面，会增加计算图卷积层的计算开销。另一方面，为了捕捉长期依赖，卷积层需要堆叠很多次。
Structural-RN提高了模型的效率，通过共现相同的RNN在同语义组中。但是，它需要人们的先验知识来分割语义组

# 7. Applications
很多的应用，文学
4类数据集

开源的图神经网络

实际的运用在各种领域

## 7.1 Datasets
1. Citation Networks 引用网络
数，作者和他们之间的引用关系，作者之间的创作关系。 无向图，相关的任务比如node classification, link prediction, node clustering tasks

三个有名的数据集
- Cora: 7个类
- Citeseer：6个类
- Pubmed： 通过TF-IDF向量来宝石
- DBLP: 计算机科学书目，参考文献

2. Social Networks
网上服务用户之间的相互关系

- BlogCatalog: bloggers, label就是兴趣
- Reddit: 无向图，论坛收集到的帖子，两个帖子之间可能包含相同用户者的评论
- Epinions: 多关系图，关于一件在线商品的评论，其中评论可能有多种关系，trust, distrust, coreview, co-rating

3. Chemical/Biological Graphs
化学元素的组成可以用化学原子作为点， 化学键作为边组成
> 由此来考虑产生图分类任务
graph classification performance
- NCI-1  NCI-9
- MUTAG
- D&D 蛋白质结构，属于两种
- QM9 分子式
- Tox21

4. Unstructured Graphs 无结构图
为了探索图神经网络在无结构化数据上的泛化性，k-NN使用最广泛

- MNIST: 70000 28*28
> ? convert to graph:  8-NN?graph基于像素的位置

- Wikipedia dataset
> 单词共现网络

- NewsGroup datasets
多个文档被划分为了20多种类别
> 将文档作为节点，相似性作为节点的边的权重

5. Others
- METR-LA: 交通数据库
- MOvieLens-1M: 6k用户为1million商品的排序,推荐系统的基准
- NELL： 语言学习项目，事实以及实体之间的关系
![](datasets.png)


## 7.2 Benchmarks 开源扩展

最常用的数据集 Cora, Pubmed, Citesser, PPI
大量的超参数，需要开源的程序才能获得同样的结果
![](methods.png)

开源代码
![](open-codes.png)

## 7.3 实际运用

GNNs 用于节点聚类，关系预测，图划分

- Computer Vision 计算机视觉
最大的应用，通过利用图神经网络探索视频中的图形结构，进行点云分类和分割，动作识别以及其他方向
在场景图的生成过程中，语义之间的关系有利于理解视觉场景背后的意义

通过给定图片场景，图生成模型检测和识别物体以及预测物体之间的语义关系

另一个应用就是通过给定的场景图生产实际的图像

每个自然语言解释为一个对象，那么就可以根据给定的语义说明类构件出图像

在点云分类和分割中，一个点云通常被看做是一个3D点。那么更觉设备探索周围的㕂，有可以识别出有问题的汽车。
为了辨别物体通过点云，将点云转化为k邻居图或者抄点图，可以用图结构来探索其拓扑结构
> 点云？？

在动作识别中，可以将人的关节分解，架构成图，从而用空间瞬时网络结构来学习人的动作行为

图像分类，人与人之间的行为，语义分割，视觉推理和问题解答

- 推荐系统
得到高质量的推荐，推荐的关键就是为一个物品的重要性为用户打分。link prediction

- 交通
graph based 空间瞬时图，路建模成点，路之间的距离建模成边，每个时间段的值建模为特征，目标来预测在某个时间段某段路的平均速度。 节省资源和节约能源

taxi-demand prediction
给出taxi需求的历史数据，位置信息，天气信息和时间特征，可以德奥一个对于每个位置的joint表达，从而来一段时间内的汽车需求

- 化学
学习化学式的结构

node 分类，图分类，图形生成
在分子图上，学习分子指纹，预测分子特性，推理蛋白质种类，合成化学物

- 其他

节目评测，时间推理，社会原因分析，恐怖时间预测等等
# 8. Future Directions
图的复杂性
## Go Deep
深度的网络结构，经过无限次的卷积，所有节点的表示可以聚集为一个单节点；所以提高层数仍然是一个好的办法
## Receptive Field
> 感受野？？？ 什么东西？
这里的意思就是一个节点的感受野就是指中心节点以及他的所有邻居节点。
邻居的数量服从一个合法的分布，每个节点可能有一个邻居，可能多成千上百个邻居
采样引入后，任何选择一个节点的感受野值得研究
## Scalability
可扩展性，大规模的图并不能扩展的很好。
主要的原因是当打包层次的图卷积时，该节点的最终装填会引起邻居的最终状态的改变，所以就会带来高复杂性的后向传播
现有的解决办法有快采样和子图训练，但是不够可扩展来解决大规模图的深度结构
## 动力学和异质性
现有的图神经网络做的都是静态的同质的图。
一方面，图的结构需要固定，另一方面，节点和边是同一来源。这两个假设不符合实际情况
比如在社会网络中，有人加入和退出
在推荐系统中，输入可能为图形或者文字
所以需要考虑新的方法


