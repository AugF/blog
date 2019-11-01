---
title: gnn-parallel-ggsnn-code-read
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-10-10 21:16:00
tags:
- gnn-parallel
categories:
- gnn-parallel
---

## 1. torch lua

th test.lua to test all the modules in the ggnn and run libraries

1. go into "babi/data", run'bash get_10_fold_data.sh" to get 10 folds of bAbI data for 5 tasks (4, 15, 16, 18, 19) and do some preprocessing
2. go into 'babi/data/extra_seq_tasks', run'bash generate_10_fold_data.sh' to get 10 folds of data for the two extra sequence tasks
3. go back to 'babi/' and use 'run_experiments.py' to run the GGNN/GGS-NN
4. Use 'run_rnn_baselines.py babi18 lstm' runs LSTM on bAbI task 18 for all 10 folds of data

### 目录结构
- run_rnn_baselines babi18 lstm
- run_experiments /  bash generate_10_fold_data.sh
- get_10_fold_data.sh: do some preprocessing预处理

### 实际执行
- babi4
    - run_q4()
        - babi_train.lua
            - paras 
                - nsteps: number of propagation iterations
                - learnrate: learning rate 0.01
                - momentum: ? for adam ？
                - mb: minibatch size
                - maxiters: maximum number of weight updates  100
                - printafter: save checkpoint after this amount of weight updates
                - saveafter: save checkpoint after this amount of weight updates  100
                - optim: adam
                - statedim: dimensionality of the node representations
                - evaltrain: evaluate on training set during training if set to 1
                - nthreads: set the number of threads to use with this process
                - ntrain: number of training instances 50
                - nval: number of validation instances 50
                - annotationdim: dimensionality of the node annotations 节点annotations的维度
                - outputdir: output directory
                - mode: {selectnode, classifygraph, seqclass, shareprop_seqclass} selectnode
                - datafile: should contain lists of edges and questions in standard format
                - seed: random seed.
            ```
            preprocess其实就是edges上的属性，点的属性
            color = require 'trepl.colorize'
            babi_data = require 'babi_data'
            eval_util = require 'eval_util'
            ggnn = require ',,/ggnn'
            1. prepare data
            math.randomseed(opt.seed)
            torch.manualSeed(opt.seed)
            ```
    - eval_q4()
        - babi_eval.lua

todo: 看具体是怎么做的?

### 数据预处理
    python rnn_preprocess.py processed_$fold/train/${i}_graphs.txt processed_$fold/rnn/train/${i}_tokens.txt --mode graph --nval 50

### bAbi使用
```
babi-tasks PathFinding --path-length 3 --decoys 1
babi-tasks Size --steps 3

 ./babi-tasks $i 1000 --symbolic true > symbolic_$fold/train/${i}.txt

--symbolic true 
```

## 2. pytorch

论文中总共4个数据集
task 4
task 15
task 16
task 18  只有0.3386

- main.py
    - grid(opt)
        - main(opt)
            - run(opt)
                - net = GGNN (model.py)
                - train_loss = train(epoch, train_dataloader, net, criterion, optimitzer, opt)
                - test_loss, numerator, denomitor = test(test_dataloader, net, criterion, optimizer, opt)

- model.py
    - GGNN
        - incoming and outgoing edge embedding
        - propogation model
        - output model

task4
基本数据集
- edge_types
- labels.txt: true or false
- node_ids.txt:
- question_types.txt:
- graphs.txt
    - n1 e6 n2
    - ? n1 n2 2
数据主要从graphs.txt中提取
graphs.txt中的数据
n_edges_type  2
n_tasks 4 问题的类型
n_node 4

item[1] = {task_type, _, task_output}
annotation = np.zeros([n_nodes, n_annotation_dim])  # [4, 1]
annotation[target][0] = 1
> 这里的annotation是怎么设置的？ 这样设置有什么用？

task_data_list[task_type-1].append([edge_list,annotation,task_output])
> 每次只使用一种类型的task_id进行训练

一些提前设置的参数
-D: denmensions 50
-H: hidden layer size 50

然后进行变换的参数是
bAbIDataloder(train_dataset, batch_size, shuffle, num_workers)
batch_size = 1,   num_workers = 2


### pytorch数据处理的通路

基于task4

1. 原始含义->graphs.txt
    - 单条数据
        > 1 1 2: node_1 e_type node_2  fact
        > 3 1 1    fact
        > ? 1 1 2: ? qustion_node qustion_type(<=edge_type) question_target
    - 根据quetsion_type, 每次指示某个task_type进行运行GGNN模型
    - 统计数据
        - n_edges_type 2
        - n_node 4
        - n_tasks 4
    - 返回结果
        - edges: (node_1, edges, node_2)
        - annotation:  [n_nodes, n_annotation_dim]
            > 这里n_annotation_dim由用户自己指定，这里为1
            > question_node对应位为1
            > 这里值得注意  n_annotation_dim=2,   结果为`[[1, 0, 0, 0], [0, 0, 0, 0]]`, 只指示`[question_node - 1][0]`为1
        - target: question_target
        > 

2. dataset -> dataloader(batch_size=1, shuffle=True, num_workers=2):
    > 猜想是转换为邻接矩阵，或者其他？  介绍的是 产生multi-process iterable
    - dataset
        ```
        <class 'list'>: [[[1, 1, 2], [3, 1, 1]], array([[1.],
       [0.],
       [0.],
       [0.]]), 2]
       <class 'list'>: [[[3, 1, 2], [1, 1, 3]], array([[0.],
       [0.],
       [1.],
       [0.]]), 2]
        ```
    - dataloader
        > num_samples 15???
        ```
        [[[0., 0., 1., 0., 0., 0., 0., 0., 0., 1., 0., 0., 0., 0., 0., 0.],
         [1., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0., 0., 0., 0., 1., 0., 0., 0., 0., 0., 0., 0.],
         [0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0., 0.]]]
         // [4, 16, 1]
         [[[1.],
         [0.],
         [0.],
         [0.]]]
         
         [1]
        ```
        - adj_matrix:  (1, 4, 16):  [A_in, A_out]
        - annotation (1, 4, 1)
            > 这里前面的1是与那个有关
        - target:  按索引开始排列，-1, 从0开始 (1,)
    - train.py对代码使用
        - padding: (n_nodes, n_nodes, opt.state_dim - opt.annotation_dim)  (1, 4, 3)
            > len(): 取的一直都是第一位
            > ? 有什么用
            > opt.state_dim: 论文中有提到的关于每个节点要设置多少个的东西
            > 所以这个是关于前面的补充？？
        - init_input = (annotation, padding)
        - output = net(init_input, annotation, adj_matrix)
            > shape: [1, 4],  估计里面存储的是softmax值
            - GGNN mode: Select node???
                - forward(prop_state, annotation, A)
                    - prop_state: (1,4,3)
                    - annotation: (1,4,1)
                    - A: (1, 4, 16)
                    > A的猜测对了，但是in_states, out_states还没检查

    - main.py中相关参数
        - opt.state_dim: default 4
        - opt.n_steps: propogation: 4


pytorch用法小结
torch.cat((x, x, x), 1)
torch.stack((x, x, x), 2) 进行数据的封装
transpose(x, 0, 1) 进行转置
> https://zhuanlan.zhihu.com/p/64551412
> 
view(1,6) 按照所想的内容进行填充
> view(-1, 1, 2): 如果为-1表示这个位置由其他位置的数字来推断
nn.Linear(): 权重自己给定
## torch

1. ggnn.NodeSelectionGGNN(state_dim, annotation_dim, prop_net_h_sizes, output_net_h_sizes, n_edge_types)
    - state_dim = opt.statedim 4
    - annotation: opt 1
    - prop_net_h_sizes: {}
    - output_net_h_sizes: {state_dim} 4
        > 输出的每个节点的大小


## something new

1. 学习方式的问题

测试集和训练集分开的。
测试集和训练集

测试集上是全部关于一个图的局部的训练数据。

所以实际上对于每个图的获取方式都不一样

训练数据是原本图上的一部分

2. edge-type训练还是question_type训练

edge-type不对，这里是边类型直接进行处理了

question_id 进行训练

这里question_id实际上是边的type类型，跟之前的理解是一样的

3. sequence 观测点的想法: 对的，刚刚看了下论文，发现确实是这样的！ 应该是中间的标注能够知道。

4. 是否有初始维度之说。 对的，没错！！！

5. 训练模式
> 这又回到了传统的训练方式，不过是借助了图的模型而已。还是进行一个图形一个图形地进行训练

6. 跟原始的有什么区别，原始的一大串进行考虑，结果发现怎么根据边来传播，发现这样做是最成功的
## 思考

1. 普通的机器学习 vs 图神经网络

普通机器学习过程即学习一个函数，然后有很多数据都是关于这个函数的。
然后，用一个数据会产生loss, 然后根据loss进行反向传播，更新梯度。
然后，不断地用多组数据不停地进行训练。

图神经网络
一部分图的数据，一部分图的标签来模拟常见操作。

不对是一组数据进行学习的过程。


2. 导数更新
对于传统的机器学习。导数更新实际上是由公式算出来的导数，然后导数更新公式上赋值上一堆权重。问题就是如何进行更新。

对于一般情况来说，如果是一个训练数据直接使用一个数据进行训练即可。如果是多个训练数据，则loss来进行更新

3. 分析
相比于传统的图数据处理方式，GNN, GG-NN等模型可以有效地利用图的拓扑结构信息。

这些模型在每个传播时间步都需要展开所有的节点，因而这些模型也可以用于处理各种图（包括有向图、无向图、有环图、无环图等）。

而且有以下问题：
> 1. 在每个时间步都要展开图中所有节点，效率降低；
> 2. GG-NN将传播时间固定，可能只会得到有效的信息。

4. 总结
> 实际是每步即根据出度和入度矩阵进行标签传播，然后每一步都是在图上，从一个节点走到另外的节点。但是这样图上的信息传输也因此受到了约束。
> 而且只是对特定的点给予信息，这意味着只能这些点的信息进行向前传播


## 10.28总结

对GGNN总结主要如下：
所谓的A_in, 实际上就是每个节点新一轮的表示是按照当前边的in边，将in边的节点的值进行加和，从而得到最终的结果。
> 详细可见矩阵相乘的运算 
