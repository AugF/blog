---
title: daily note 11-28
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-11-28 21:03:29
tags:
categories:
---

## A. 今日遗留任务：

1. 查看GAT代码，思考是否能够直接用tensorflow multiplication来表示。 回看GGCN论文
https://github.com/Diego999/pyGAT/blob/master/layers.py
GAT论文还提出一个很有意思的点，就是tensorflow只能做batch类似的操作，不能做graph并行化

- pytorch
torch.mul(a,b): 元素相乘    torch.mm(a,b): 矩阵相乘
self.W = (in_features, out_features)
self.a = (2*out_fatures, 1)
pytorch 检查成功

- tensorflow
没有检查成功！
import tensorflow as tf
node1 = tf.Tensor
node2 = tf.Tensor
addr = node1 + node2
sess = tf.Session()
print(sess.run(addr))

2. 已经安装了eigen, C++数学计算库，计划在vscode中实现
https://github.com/tensorflow/tensorflow/blob/master/tensorflow/core/kernels/sparse/mat_mul_op.cc
https://blog.csdn.net/wilsonass/article/details/90754525
多线程，相乘的感觉，没有看到具体的算法
g++ -I /path/to/eigen/ my_program.cpp -o my_program

C++ 模板程序也能理解，好像直接将这个放在对应的源文件目录下就可以
https://blog.svenhetin.com/c-ju-zhen-yun-suan-ku/

或许应该先了解稀疏矩阵的乘法是怎么？
https://www.cnblogs.com/wangkundentisy/p/9267764.html

进阶：思考如何进行

3. 再思考tensorflow中能够做什么
    > 其实tensorflow和pytorch中所表示的都是矩阵的运算，不过所给的卷积操作，还是其他的
    > NeuGraph中提到Certain algorithms不能直接用Tensorflow multiplication operations来表达是什么意思？
实际上感觉就是矩阵操作。
所以说初步结论：就是tensorflow中没有直接的函数，即矩阵相乘的运算可以一步到位。
需要借助repeat, reshape的运算。
再运用乘法

4. 阅读GraphSage论文，思考框架
Inductive 归纳   deuctive 演绎    tranductive 转导的

transductive 特殊到特殊，测试数据也是未来出现的一部分
inductive learning: 测试数据只用与训练，需要归纳到一般
https://www.zhihu.com/question/68275921

纠正一个错误：之前PPT上说inductive自己说错了。
应该是侧重于用途

感觉inductive像是一个某类问题的模板，比如排序 without task-specific
transductive像是一个具体的算法，求最大值？ 

tocheck
6. 重点：讨论AliGraph, 再细看AliGraph论文，以及后面的实验部分！
NeuGraph思考的是如何在GPU上进行加速
AliGraph是针对自己的平台的多个特性按需扩展
skip-gram论文？ word2vec
word2vec: 关键问题，如何对一个词进行表示？

power layer 原则，实际上的做法是

7. 阅读how to read a paper, sparse

8. 回顾NeuGraph
neugraph，重点：如何解决点划分，进而带来的每个块的负载均衡问题。
采用了两个策略：1

## B. 现有框架的缺点：

NeuGraph:
1. 图结构一旦确定，不能改变，不支持动态
2. 不支持GAT
3. 效果真的会比GraphSage好吗？ 看实验部分！！！ 未认真看


## 1. Paper Lists

### 1.1 Deep Learning Supported Dataflow Frameworks
TensorFlow[13], PyTorch[14], MXNet[15], CNTK[16]

### 1.2 Graph Processing Systems
Pregel[1], GraphLab[2], PowerGraph[3], GraphX[4]

### 1.3 Graph Neural Networks
**1. Survey**
A Comprehensive Survey on Graph Neural Networks[25]

**2. Graph Convolution Networks**
1stChebNet[26],  GGNN[27]

**3. Others**
CommNet[28]

### 1.4 GNNs Modeling Frameworks 
GraphSAGE[17], MPNN[18], GNBlock[19]

### 1.5 GNNs Parallel Systems
NeuGraph[29],  AliGraph[30]

### 1.6 Non-GNNs Parallel Works
AMPNet[23],  BPT-CNN[24]

### 1.7 Optimizations

**1. Graph Layout, Sequential Data Access, and Secondary Storage**
GraphChi[5], Grace[6], FlashGraph[7], XStream[8], Chaos[9]

**2. Ditributed Shared Memory**
Grappa[10]

**3. NUMA-Awareness, Scheduling, and Graph Partitioning**
PowerLyra[11], BiGraph[12]

**4. Sparse Works**
Sparse Deep Neural Network Graph Challenge[22]

### 1.8 Others

**1. Bridge the Gap between Graph and Traditional Machine Learning Computation**
TuX2[20]

**2. Introduce the Vertex-Centric Programming Model into Dynamic Neural Networks**
Cavs[21]

### 1.9 AliGraph


## 2. References

[1]  Elmagarmid, A. K., Agrawal, D., & Association for Computing Machinery. Special Interest Group on Management of Data. (n.d.). Pregel: A System for Large-Scale Graph Processing.
[2]  Low, Y., Gonzalez, J., Kyrola, A., Bickson, D., Guestrin, C., & Hellerstein, J. M. (2150). Distributed GraphLab: A Framework for Machine Learning and Data Mining in the Cloud.
[3]  Gonzalez, J. E., Low, Y., Gu, H., Bickson, D., & Guestrin, C. (n.d.). PowerGraph: Distributed Graph-Parallel Computation on Natural Graphs (Vol. 12).
[4]  Flinn, J., Levy, H., ACM Special Interest Group in Operating Systems., USENIX Association, & ACM Digital Library. (n.d.). GraphX: Graph Processing in a Distributed Dataflow Framework.
[5]  Kyrola, A., Blelloch, G., & Guestrin, C. (n.d.). GraphChi: Large-Scale Graph Computation on Just a PC.
[6]  Prabhakaran, V., Wu, M., & Weng, X. (2012). Managing Large Graphs on Multi-Cores With Graph Awareness Design for a Graph Management System. USENIX Annual Technical Conference.
[7]  Zheng, D., Mhembere, D., Burns, R., Vogelstein, J., Priebe, C. E., & Szalay, A. S. (2014). FlashGraph: Processing Billion-Node Graphs on an Array of Commodity SSDs. 
[8]  Roy, A., Mihailovic, I., & Zwaenepoel, W. (2013). X-Stream: Edge-centric graph processing using streaming partitions. SOSP 2013 - Proceedings of the 24th ACM Symposium on Operating Systems Principles, 472–488.
[9]  Roy, A., Bindschaedler, L., Malicevic, J., & Zwaenepoel, W. (2015). Chaos: Scale-out graph processing from secondary storage. SOSP 2015 - Proceedings of the 25th ACM Symposium on Operating Systems Principles, 410–424.
[10]  Nelson, J., Holt, B., Myers, B., Briggs, P., Ceze, L., Kahan, S., & Oskin, M. (2015). Latency-Tolerant Software Distributed Shared Memory. USENIX Annual Technical Conference (ATC)
[11]  Chen, R., Shi, J., Chen, Y., Zang, B., Guan, H., & Chen, H. (2018). PowerLyra: Differentiated graph computation and partitioning on skewed graphs. ACM Transactions on Parallel Computing, 5(3).
[12]  Chen, R., Shi, J. X., Chen, H. B., & Zang, B. Y. (2015). Bipartite-Oriented Distributed Graph Partitioning for Big Learning. Journal of Computer Science and Technology, 30(1), 20–29. 
[13]  Abadi, M., Barham, P., Chen, J., Chen, Z., Davis, A., Dean, J., … Zheng, X. (2016). TensorFlow: A system for large-scale machine learning. 
[14]  PyTorch. http://pytorch.org, Retrieved January,2019.
[15]  Chen, T., Li, M., Li, Y., Lin, M., Wang, N., Wang, M., … Alberta, M. U. (n.d.). MXNet: A Flexible and Efficient Machine Learning Library for Heterogeneous Distributed Systems.
[16]  Yu, D., Eversole, A., Seltzer, M., Yao, K., Huang, Z., Guenter, B., … Slaney, M. (2015). An Introduction to Computational Networks and the Computational Network Toolkit. In Microsoft Technical Report.
[17]  Hamilton, W. L., Ying, R., & Leskovec, J. (2017). Inductive representation learning on large graphs. Advances in Neural Information Processing Systems, 2017-Decem(Nips), 1025–1035.
[18]  Gilmer, J., Schoenholz, S. S., Riley, P. F., Vinyals, O., & Dahl, G. E. (2017). Neural message passing for quantum chemistry. 34th International Conference on Machine Learning, ICML 2017, 3, 2053–2070.
[19]  Battaglia, P. W., Hamrick, J. B., Bapst, V., Sanchez-Gonzalez, A., Zambaldi, V., Malinowski, M., … Pascanu, R. (2018). Relational inductive biases, deep learning, and graph networks. 1–40.
[20]  Xiao, W., Xue, J., Miao, Y., Li, Z., Chen, C., Wu, M., … Zhou, L. (2017). Tux2: Distributed Graph Computation for Machine Learning. Nsdi ’17, 669–682. 
[21]  Xu, S., Zhang, H., Neubig, G., Dai, W., Kim, J. K., Deng, Z., … Xing, E. P. (2018). Cavs: An Efficient Runtime System for Dynamic Neural Networks. 2018 {USENIX} Annual Technical Conference ({USENIX} {ATC} 18), 937–950. 
[22]  Kepner, J., Alford, S., Gadepally, V., Jones, M., Milechin, L., Robinett, R., & Samsi, S. (n.d.). Sparse Deep Neural Network Graph Challenge. 
[23]  Gaunt, A. L., Johnson, M. A., Riechert, M., Tarlow, D., Tomioka, R., Vytiniotis, D., & Webster, S. (2017). AMPNet: Asynchronous Model-Parallel Training for Dynamic Neural Networks.
[24]  Chen, J., Li, K., Member, S., Bilal, K., Zhou, X., Li, K., & Yu, P. S. (n.d.). A Bi-layered Parallel Training Architecture for Large-scale Convolutional Neural Networks.
[25]  Wu, Z., Pan, S., Chen, F., Long, G., Zhang, C., & Yu, P. S. (2019). A Comprehensive Survey on Graph Neural Networks. X(X), 1–22. 
[26]  Kipf, T. N., & Welling, M. (n.d.). SEMI-SUPERVISED CLASSIFICATION WITH GRAPH CONVOLUTIONAL NETWORKS.
[27]  Li, Y., Tarlow, D., Brockschmidt, M., & Zemel, R. (2015). Gated Graph Sequence Neural Networks. (1), 1–20.
[28]  Sukhbaatar, S., Szlam, A., & Fergus, R. (2016). Learning multiagent communication with backpropagation. Advances in Neural Information Processing Systems, 2252–2260.
[29]  Ma, L., Yang, Z., Miao, Y., Xue, J., Wu, M., Zhou, L., & Dai, Y. (2019). NeuGraph: Parallel Deep Neural Network Computation on Large Graphs. 2019 USENIX Annual Technical Conference (USENIX ATC 19), 443–458.
[30]  Zhu, R., Zhao, K., Yang, H., Lin, W., Zhou, C., Ai, B., … Zhou, J. (2018). AliGraph: A comprehensive graph neural network platform. Proceedings of the VLDB Endowment, 12(12), 2094–2105.