---
title: paper-read-graphsage
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-12-06 15:22:24
tags:
- gnn
categories:
- gnn
---

Overview:

stochastic generalization of graph convolutions, and it is useful for massive, dynamic graphs that contain rich feature information.


看自身的PPT, 实际上关于k的提法，从1,2,...,k其他是对于每个节点。
比如说图上的每个节点，先分别做一跳的表示。
不断地一起共同的做，最终可以发现实际上所求的目标就最终表示是k跳来决定的。

关键表达式其实就只有那一个。

为什么inductive?
实际上感觉是在图上把问题给固定下来了，也就是说实际上，来一个性的点之后，直接按照之前的所有的函数和模式，学习新的表示即可。
> 此时的表示是基于之前训练得到的新的表示? 还是旧的表示?
> 之前的学习是学习对应的函数和参数，所以说?

> 那么对于未知节点的表示，实际上又是如何查找和运行的呢?

hierarchical 层次

hierarchical pool: 层次遍历。
> 如果将gnn适用于对整个图分类。

Characterize GNN's discriminative power

## paper

### dataset
#### Ciatation data 
predicting paper subject categories.
    six biology-related fields for 2000-2005.  node lable: six

n = 302421,  fix_size = 9.15
undirected citation graph dataste.

train: 2000-2004
2005 test.  30% for validation

features:  node degrees and processed the paper abstracts 
300-dimensional word vectors.

#### Reddit data

which community different Reddit posts belong to.

users post and comment on content in different topical communities.

node label: the community.

sampled 50 large communities.  post-post graph.

> in the month of September, 2014.

n=232965,  fix_size=492
first 20 days for training and remaining for testing (0.3 for validation) 

features: 300-dimensional GloVe CommonCrawl word vectors.

each post concatenated 1. the average embedding of the post title.
2. the average embedding of all the post' comments 3. the post's score. 4. the number of comments made on th post

#### Protein-protein interactions

the task of generalizing across graphs.  requires  learning about node roles rather than community structure.

protein roles, in terms of their cellular functions from gene ontology.
我们未来了解节点的角色，即蛋白质的作用是什么, protein. 使用基因本体论为它们做标记。

节点与节点之间交互会构成人体的不同组织

various protein-protein interaction graphs.

node: protein
graph: human tissue.

features: positional gene sets, motif gene sets and immuological signatures
labels: gene ontology sets

n=2373, fix_size = 28.8

### 5. Theoretical analysis

为了探究GraphSage如何学习图结构，尽管它本质上可能是基于功能。
作为案例研究，我们考虑了graphsage是否可以学习预测节点的聚类稀疏，即节点1跳领域内闭合的三角形比例。
> 反映在图上是连在一起的意思吗？
聚类系数是衡量节点本地淋雨聚类程度的一种流行度量，它是许多更复杂结构图案的基础。

我们可以证明算法1能够将聚类稀疏近似为任意精度

Theorem 1?: 公式表达

对于每个图，都有一个算法1的参数设置，这样，如果每个节点的特征都不同，则它可以将改图的聚类系数近似为人影精度。

即使从绝对连续的随机分布中采样节点特征输入，GraphSage也可以了解局部图的结构。

证明背后的基本思想是，
如果每个节点都有唯一的特征表示，则我们可以学习将节点映射到指标向量并识别节点领域。
定理1的证明依赖于池聚合器的某些属性，所以更优
### Appendices

#### Algorithm 2
首先第一步预处理，
K轮：  Bk=B
B(k-1) = Bk 加上 u的点的邻居
B(k-2) = B(k-1) 加上所有点的邻居
> ?  反着来违反直觉，然后k=2是，即采样了S2, 又采样了S1*S2 2-hop neighbors.
> 解释说这里其实是采样过程而已

12-13 解释说运算肯定是只包括当前所需要的运算

#### Dataset

#### Deatails
1. Hyperparameter selection
Random Walk:
50 random walks of length 5 from each node in order to obtain the pairs needed for the unsupervised loss.
Python,  Perozzi[28]

Logistic regression model
SGDCClassifier from the scikit-learn Python package[26], with default settings.

Hyperparameter selection
DeepWalk:  0.01, 0.001, 0.0001 initial learning rates. 2*1e-6, 2*1e-7, 2*1e-8 unsupervised model

big 1024,  small 512

All models user rectified linear units
All the unsupervised GraphSAGE models and DeepWalk used 20 negative sample with context distributaion smoothing over node degress using a smoothing parameter of 0.75.

rates: 0.2, 0.4, 0.8

batch sizes 512,  batch size 64

2. Hardware
4 NVIDIA Titan X Pascal GPUS (12Gb of RAM at 10Gbps speed), 16 Intel Xeon CPUS(E5-2623 v4 @ 2.60GHz)

3 days in a shred resource setting.
> 查一下这些硬件

Titan X GPU:  4-7 days  full resources were dedicated

3. Notes on the DeepWalk implementation
DeepWalk is also equivalent to the node2vec model with p=q=1

4. Notes on neighborhood sampling:
subsample edges so that no nodel has degree large than 128.
we only sample at most 25 neighbors per node.
downsampling allows us to store neighborhood information as dense adjacency list, improves computational efficiency.

Reddit data, downsampled the edges of the original graph as a pre-processing step, since the original graph is extremely dense.
## code

### tensorflow

stochastic generalization of graph convolutions.


GraphSage now has better support on smaller, static gaphs,  don't have node features.   'identity features"

task of inductive generalization(generating embeddings for nodes that were not present during training)

identity features: increase the runtime. potentially increase performance(at the usual risk of overfitting)


GraphSage is intended for use on large graphs >100000 nodes.

example_data: ppl


1. running the code
if your benchmark does not require generalizing to unseen data,  --identity_dim  ?flag to a value int ht range [64, 256]
> 目标是不需要inductive? 没错吧，那么不应该是也可以用到自身特征吗？
make the model embed unique node ids as attributes.
increase the runtime and number of parameters , performance.
> node ids, 节点标号作为属性，什么鬼？  
set this flag and not try to pass dense one-hot vectors as featres(dut to sparsity)
> 单独抽出来可以理解
the "dimension" of identity features specifies how many parameter there are per node in the sparse identity-feature lookup table.
> 这个维度是怎么展现的？不精确的？

example_unsupervised.sh

set a samll max iteration， very near convergence ?

example_supervised.sh

for PPI data
> mulit-output? 除了节点编号，还要输出什么？懂了！，这里的意思是individual nodes to belong to multiple classes. --sigmoid,   defalut: one-hot


2. Input format
--train_prefix: specifies the following data files.
X-G.json: a networkx-specified file decribing the input graph. Nodes have 'val' and 'test' 表示是交叉验证集和测试集的一部分
X-id_map.json: graph_node-ids映射到连续的整数
X-class_map.json: graph node ids map to classes
X-feat.npy: a numpy-stored array of node features. ordering given by id 应该是对应的整数
X-walks.txt: a text file random walk co-occurrences (one pair per line)
> 共同出现的节点标号

run random walks. 
graphsage.utils  run_walks

3. Model variants
    - graphsage_mean
    - graphsage_seq
    - graphsage_maxpol
    - graphsage_meanpool
    - gcn
    - n2v_an implementation of DeepWalk

4. logging directory
    - --base_log_dir: default to the current directory; <sunp/sunsup>--<data_prefix>/graphsage-<model_description>/
    - supervised model output F1 scores
    - unsupervied train emmeddings and store them。  val.npy,  val.txt.  5-10Gb
        > unsupervised 干什么？ 再看论文， use to the downstream machine learning applications.  `eval_scripts`

### 代码

1. tesorflow: 代码未看，速度快
python -m graphsage.supervised_train --train_prefix ./example_data/ppi --model graphsage_mean --sigmoid

python -m graphsage.unsupervised_train --train_prefix ./example_data/ppi --model graphsage_mean --max_total_steps 1000 --validate_iter 10

> in order to learn useful, predictive representations in a fully unsupervised setting, use graph-based loss function. 
> 因为是无监督，所以loss是自己定义出来的，不需要真实标记参与进来
> 修改PPT, 这里应该是无监督学习


与GCN最大的区别，在于多了concat操作。

samp_neighs = [sample_neigh + set([node[i]])]

代码还有一个mock的bug未解决

注意到这里其实有好几个loss functions

2. pytorch: 代码已看，速度快
总结：总依据是原本的算法。
word embedding特征,  cora, pubmed引用数据集是这么做的！
其他建议

每轮是batch, 训练数据的读入。
256, 1024, 然后random.shuffle.
下次再参与训练。

在这里的特征使用了word embedding
gcn: 81.5 4s, 训练方式是所有
graphsage: 0.874
两个进行对比，不同在哪里！

问题？
graphsage说将gcn改变为inductive, 就是这里进行改变吗？

有证明sample取不同的阈值，可以达到不同的精确度

## 论文查找

论文： Hamilton W, Ying Z, Leskovec J (2017) Inductive Representation Learning on Large Graphs

### parallel 215
1. Neugraph
2. parallel computation of graph embedding:
! large graphs. framwork for parallel computation of a graph embedding using a cluster of compute nodes with resource constraints.
propose a new way to evaluate the quality of graph embeddings that is independent of a specific inference.
computation scales well, while largely maintaining the embedding quality.

3. Accurate, Efficient and Scalable Graph Embedding
>  a major challenge is to reduce the complexity of layered GCNs and make them parallelizable and scalable on very large graphs

4. Towards Efficient Large-Scale Graph Neural Network Computing


3篇文章是互不影响的关系


interesting

1. Deep Inductive Graph Representation Learning

2. large-Scale learnable graph convolutional networks
> subgraph train

3. Generalizable Resource Allocation in Stream Processing via Deep Reinforcement Learning
> RL

4. Deep Neural Representation Learning on Dynamic Graphs via Self-Attention Networks
>  动态图

### distributed


### cluster system~~