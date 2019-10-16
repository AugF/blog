---
title: semi-paper code read
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-12 12:47:23
tags:
- gnn
- semi-gnn
categories:
- [gnn, semi-gnn]
---


## 论文1
https://github.com/tkipf/gcn

Data
- n*n   n*d  d=c:channels  e=f:classes

1. gcn 文件组织结构
    - readme.md
        - data:  d=c: chanels   e=f:classes
            - citation network data:  cora, citeseer, pubmed
            - nell:  revisiting semi-supervised learning with graph embedding
        - model: gcn,  gcn_cheby, dense
    - gcn
        - data
            - .tx  the feature vectors of test instances
            - .ty  the one-hot labels
            - .index   the indices of test instances in graph,   inductive setting
            - .x :  the feature vectors of the labeled training instances 
            - .y :  the labeled
            - .allx: all
            - .ally
        - inits.py
            - uniform,  glorot: [input_dim, output_dim],  zeros, ones
        - metrics.py
            - cross_entropy + accuary
        - models.py
            - gcn: semi-superised 
            - gcn_cheby: convolutional
            - dense: basic multi-layer perceptron
            - Model
                > self.layers[0], 第一层的layer. 对第一层中的var的值，进行相加，这里的var只有weights, bias.  进行乘以固定系数相加
                > num_features_nonzero: dropout的方法也不同
                - init
                    - name, logging
                    - vars, placeholdres
                    - layers, activations
                    - inputs, outputs
                    - loss, accuary
                    - optimizer, opt_op
                - build:   tf.variable_scope(self.name)
                    - build sequential layer model
                    - store model variables for easy access
                    - build metrics:  weight_decay: 5e-4: weight for L2 loss on embedding matrix. ???
                - predict
                - _loss
                - _accuracy
                - save
                - load
            - MLP
                - __init__:  inputs: features,  input_dim, output_dim, placeholders, optimizer, build
                - _loss: 变量的l2 loss???? += 交叉熵
                - _build;  Dense
            - GCN
                - _build: GraphConvolution
        - utils.py
            - load_data
        - layers.py
            - get_layer_uid: 全场唯一uid
            - sparse_dropout: dropout for sparse tensors????
            - Layer
                - init: name, vars, logging, sparse_inputs
                - _call: return inputs
                - __call: add inputs, outputs ??? tf.summary.histogram()?// 为了方便展示直方图?
                - _log_vars:  add vars in histogram
            - Dense(Layer)
                - __init:  input_dim, output_dim, placeholders, dropout=0., sparse_inputs, act, bias=false, featureless=false  glorot()
                - _call
                    - dropout:
                    - transform:  是否使用稀疏矩阵相乘
                    - bias:  self.vars['bias'] 没有输入的，只有输出，在隐藏层中，对结果产生影响
            - GraphConvolution: graph convolution layer(Layer)
                - act?  support?  bias?   featureless?  act: tensorflow activation
                - init
                    - support 变量的个数 weights_i:  glorot   bias=zeros
                - _call
                    - convlve ??
        - train.py
            - flags
                > dataset, model, learning_rate, epochs,, hidden1-16, dropout
                - weight_decay: weight for l2 loss on embedding matrix
                - early_stopping
                - max_degree:  Chebyshev 不等式的阶
            - placeholders
                - support: [preprocess_adj(adj)]
                    > 为什么要这么做? 因为gcn_cheby模型中每层的A实际上是不一样的
                - features: 
                - labels: 
                - labels_mask: 
                - dropout: 注意稀疏矩阵和稠密矩阵 dropout方法还不一样 
                - num_feature_nonzero: 
            
corn
    - x:  140,1433  tx: 1000,1433  ty: 1000, 7
    - allx: 1708  1433  ? 为何总数不相等
    
    - (allx,tx) 说明这两个不相等 

### 论文2
https://github.com/kimiyoung/planetoid

- test_ind.py:  induce model 的测试
- test_trans.py:  transive model的测试
- ind_model.py:  model   add_data  build  gen_train_inst   gen_graph gen_lable_graph