---
title: deeplearning cs231n
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:23:33
tags:
- deeplearning
- stanford
- cs231n
categories:
- [deeplearning, cs231n]
---
#  1. Introduction

1. A brief history of computer vision
    - creatures -> vision
    - camera
    - what is the vision processing
        - Original picture
        - differntiated picture
        - Feature points selected
    - MIT research
        ![](asserts/1-stagesOfvisualRepresentation.png)
        - Inpute image
            - preceived intensities
        - Edge image
            - zero crossing, blobs, edges, bars, ends, virtual lines
        - 2 1/2-D sketch
            - local surface orentation
        - 3D 
    - how to recognize and represent the world
    - image segmentation: Jitendra Malik
    - face detection
    - 1999-2000, momentum动量, ML,  adaBoost algorithm to do real-time face detection, 2001
    - 2006, digital camera a real-time face detector
    - SIFT feature: match and the entire object   due to camera angles, occlusion, viewppoint, lighting
    - Spatial pyramid matching: from different parts of the image and in different resolutions and put them together in a feature 
    - HoG Histogrm of Gradients
    - imagenet
    - SVM, AdaBoost,  overfiting: not enought training data, cannot generalize.  benchmarking  WordNet
    - 2009  ImageNet Large-Scale Visual Recognition Challenge
2. CS231n overview
    - Aims
        - image classification
        - object detection
        - action classification
        - image captioning
    - Convolutional Neural Neteworks
    - object relationships
    - object attributes
    - Our philosophy
        - Thorough and Detailed
        - Practical
        - State of the art
        - Fun
    
# 2. Image Classification pipeline

python numpy tutorial

https://github.com/cs231n/cs231n.github.io/blob/master/python-numpy-tutorial.md

gce~tutoria ?
google compute engine

1. cluster

2. attempts have been made
    - find edges
    - find corners ->, <-, 
    > not scalerable

3. Neareast Neighbor
    - train: memorize all data and labels O(1)
    - predict: predict the label of the most similar training image O(N)
    > noisy  spurious
    ![](asserts/2-1nn.png)
    > just training data, 把所有测试数据可能的分类填上去
    > k-nearest neighbors
    ![](asserts/2-knn.png)
    > the white place 可能选哪个都可以，随机才

L1: coordinate system is important
> you know what the coordinate system meaning

![](asserts/2-knn-distance_metric.png )
numpy的基本运算

http://vision.stanford.edu/teaching/cs231n-demos/knn/

why 图像是那样的????
> PRML必读

- Setting Hyperparameters

pick the best training hyperparameters performs on validation?
and run once on the test data, then write in the paper

cross valildation
Useful for small datasets, but not used too frequently in deep learning

有个点自己理解错了，其实是看交叉验证集上的损失怎么样？如果交叉验证集上的损失没有连续操作多少次上升？ 或者达到终止条件，则选择该超参数


不过，k-cross-validation的意思是用同样的超参数训练多次，然后得到验证集的平均损失，然后更新一次吗？

因为本身更新公式是作用在损失函数上的

validation to check how well

same methodology for collecting the data,  go and partition it randomly

时间序列分割
shift the problem

![](asserts/2-k-cross-validation.png)

L2 distance is really not doing a very good job

cover space densely  exponential growth 
traning examples?

knn不适用与图像分类
https://blog.csdn.net/chaipp0607/article/details/77915298

- 很差的测试效率
- 整个图像的水平距离度量可能非常不直观

> 计算距离的对象不可能是像素值，因为他不能从本质上反映出人对图像的认知，比如把一张图进行位移，人觉得是一类，但是计算结果距离很大。
所以knn计算的应该是一些描述子生成的特征。

knn总结
1. 对于样本不平均问题，knn相比于其他监督学习算法容忍度更低。
2. 计算和存储开销大，在线学习效率低。

改进
1. 解决样本不平衡问题
2. 提高分类效率

![](asserts/2-hard-cases-for-alinearclassifier.png)

mutlimodal data: different regions of space

229

# 3. some concepts
## 3.1 loss
- SVM loss:  $L_i = \sum_{j \ne y_i} max(s_j - s_{y_i} +1)$
$L = \frac{1}{N} \sum_{i=1}^N L_i$

$max(1-yf(x))$: 在分类分割线附近进行鼓励
> care the correct is much greater than the incorrect scores

loss function; how bad are different mistakes
square:  bad, very, very bad
hinge:  a litte wrong,  a better wrong

损失函数总结
https://zhuanlan.zhihu.com/p/58883095

> 设置W, 来查看Loss

W, 2W  the zero loss


## 3.2 regularization term: encourages the model to somehow pick a simple W $R(W)$
![](asserts/3-regularization.png)

L1 encouraging sparsity in this matrix W.
w1: 0.4, 0, 0.4, 0
w2: 0.2, 0.2, 0.2, 0.2
x:1,1,1,1

L1:w1   L2:w2
> 正则化项是在原本目标得到的函数的基础上，按照你想要的某种特性去构造

minimize  -log P(Y=yi| X=xi)
multi-SVM: softmax loss 
> min=zero,  max is infinity

> 当出现错误的数据，如何处理？ 0应该处理为一个非常小的数
> debuging:  minus log of one over C

- hinge-loss vs softmax loss
> SVM-loss: margin  between different classes,  if the data point over the bar, they don't care about it anymore if they are correct.
> softmax-loss: drive probabiltity mass all the way to one, pile the all classes, because log loss, it tends to push correct class to 0,  imcorrect classes to infinity.
continually to improve the correct classes.

## 3.3 Optimization

1. A first very bad idea solution: Random search

cifar-10  15.5%

2. fllow the slope
> scalar 标量

如何计算梯度？
- 前向传播，然后计算一个新值，然后得到  dW
![](asserts/3-gradient.png)

> debugging 

https://vision.stanford.edu/teaching/cs231n-demos/linear-classify

calclus 微分

![](asserts/3-gradient-ex.png)

add gate: gradient distributor
max gate: gradient router
mul gate: gradient switcher

back to a node is add

![](asserts/3-Jacobian.png)
4096*4096? Jacobian matrix:  diagonal matrix 

step 1
![](asserts/3-gradient-vectorized-ex.png)


step 2
![](asserts/3-gradient-vectorized-ex-2.png)
> Always check: the gradient with respect to a variable should have the same shape as the variable.


![](asserts/3-forward_backward-api.png)
https://github.com/BVLC/caffe/blob/master/src/caffe/layers/clip_layer.cpp
> caffer layers

## 3.4 layers kinds
http://tutorial.caffe.berkeleyvision.org/tutorial/layers.html

- vision layers
    - convolution
    - pooling
    - local response normalization(lrn)
    - im2col
- loss layers
    - softmax
    - sum-of-squares/euclidean
    - hinge/margin
    - sigmoid cross-entropy
    - accuracy and top-k
- Activation/Neuron Layers
    - ReLU/Rectified-Linear and Leaky-ReLU
    - Sigmoid  $y = \frac {1}{1 + e^{-x}}$
        > https://github.com/BVLC/caffe/blob/master/src/caffe/layers/sigmoid_layer.cpp
    - TanH/Hyperbolic Tangent
    - Absolute Value
    - Power
    - BNLL
- Data Layers
    - In-Memory
    - HDF5 Input
    - Images
    - Windows
    - Dummy
- Common Layers
    - Innner Product
    - Splitting
    - Flattening
    - Concatenation
    - Slicing
    - Elemetwise Oprations
    - Argmax
    - Softmax
    - Mean-Variance Normalization

examples
![](asserts/3-svm.png)

summary
![](asserts/3-summary.png)
> upstream gradient multipy local gradient

> 左乘优先
non-linearities

question: templates, in fact, you don't have to care about it, the only thing you concerned is the scores

![](asserts/3-neural_networks.png)

> Assignment2: Writing a 2-layer net

## 3.5 Activation functions
![](asserts/3-activation_functions.png)

> callled 2-layer neural net

matrix form, matrix-vector form

Summary
- arrange neurons into fully-connected layers
- the abstraction of layer has the nice property that it allows us to use efficient vectorized code(e.g. matrix multiplies)
- neural networks are not really neural
- next time: convolutional neural networks

# 4. Convolutional Neural Networks

Deep Learning history
- 1957: Perceptron, Frank Rosenblatt
- 1960: Adaline/Madaline
> stack these linear layer to multi-layers, no back-propagation or other rules
- 1986: Rumelhart et al. First time back-propagation became popular
- 2006: Reinvigorated research in Deep Learning
> carefully-initialized   pre-training stage 
- 2012, first strongest results using for speech recognition


A bit of history: CNN
- 1959 electrical signal from brain
> topographical mapping 地形学
- Hierarchical organization:
> simple cells -> complex cells -> hypercomplex cells
- 1980 complex cells->perform pooling
- 1998 gradient-base document recognition
- 2012, AlexNet
- Detection, Segmentation  R-CNN
reinforcement learning

notation
should rorate 180 
how to slide this over all the spatial locations
> 卷积层得到的结果是什么  
> 3 filter??,  6 filters??
over the image spatially
we're sampling the edges and corners less than the other localtions

> 试图去理解每一步的过程还有每一个公式的含义

one filter-> one activation map

we call these
![](asserts/4-convolutional.png)


![](asserts/4-output_size.png)

Output size:
(N-f)/stride + 1

> if we use common to zero pad the border, we will get 7*7 output

![](asserts/4-outputsize-ex.png)
> what will channels do?
5*5*3 + 1=76 params(+1 for bias) => 76*10=760 params

![](asserts/4-outputsize_summary.png)


> 概念辨析
- 向量的乘法
> 点集，标量积得到内积； 叉乘，结果为向量，向量积
- 矩阵乘法
> multiply: 普通的乘法
> Hadamard product, 逐点乘法
> Kronecker product:  tensor product

conv layer in torch
> spatial convolution
-----
torch, pytorch
> torch能够放在GPU中加速计算，前提是有合适的GPU时，使用起来跟numpy来说差别不大

- torch
https://github.com/torch/nn/blob/master/doc/convolution.md#nn.SpatialConvolution

- caffe
http://caffe.berkeleyvision.org/tutorial/layers/convolution.html

----

# 5. Training Neural Networks

Overview:
1. one time setup
    - activation functions
    - preprocessing
    - weight initilization
    - regularization
    - gradient checking
2. Training dynamics
    - babysitting the learning process
parameter updates
    - hyperparameter optimization
3. Evaluation
    - model ensembles

## 5.1 Activation Functions
1. sigmoid 
    - introduction
        - squashes numbers to range[0,1]
        > [-5, 5]变化明显，其他位置变化就有问题了 [-10, 10]=0
        > df = f(1-f)
        > 梯度就是函数图像的直线，永远不要忘了最直观的理解
        - historycally popluar since they have nice interpretation as a saturating "firing rate" of a neuron
    - problem
        - Saturated neurons "kill" the gradients 
        - Sigmoid outputs are not zero-centered
        > 因为对x的范围的限制，所以不对称，同理也不能在全positive的数据上表现的很好； always the sign of the upstream gradient coming down.  inefficient weight update
        - exp() is bit compute expensive

![](asserts/5-sigmoid.png)

![](asserts/5-sigmoid-pic.png)
> avoid allways a direction

2. tanh 1991
- Squashes numbers to range[-1,1]
- zero centered (nice)
- still kills when saturated :(
![](asserts/5-tanh.png)

3. relu 2012  rectified linear unit

![](asserts/5-relu.png)

- not zero-center output
- any annoyance
hint: what is the gradient when x<0 ?

> it is fine at the begining, then at some point, it became bad and it died.

![](asserts/5-relu-problem.png)

> data cloud is just your training data.
> wx1 + wx2上进行影响啊
> people like to initialize relu neurons with slightly positive biaes(e.g. 0.01)

4. leaky relu
![](asserts/5-prelu.png)

5. elu
![](asserts/5-erelu.png) 
> how the $\alpha$ is defined

6. Maxout Neuron
![](asserts/5-max-neuron.png)

![](asserts/5-practice.png)

## 5.2 data preprocess

original data -> zero-centered data -> normalized data

> to do at the test data, too
![](asserts/5-2_preprocess_data-1.png)


![](asserts/5-2_pcaAndWhitening.png)


![](asserts/5-2_preprocess_data-practice.png)

> per-channel  RGB
>do this for entire train data,  once before training. don't do this per batch

> how sigmoid need the zero-centered data,  it only servers for the first layer.

## 5.3 Weight Intilization

Q: what will happen if W is zero?
> 1. will disappear 
> !!they will all do the same thing

- First idea: small random numbers
(gaussian with zero mean and 1e-2 standard deviation)
`W = 0.01 * np.random.randn(D, H)`
> works okay for small networks, but problems with deeper networks.

pic1 see the hidden data is
pic2: see the data distribution
> zero

> All activations become zero!!

What will W look like?

because X is small, so the W is getting more small, they're basically not updating.

- forward doing
- have gradient flows coming down
> for each line, we chain these weight

cs231n: 深度视觉识别课程P6