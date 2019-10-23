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



