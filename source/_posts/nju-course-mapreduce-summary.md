---
title: nju-course mapreduce summary
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:05:54
tags:
- nju
- course
- mapreduce
categories:
- [course, nju, mapreduce]
---

## ch1 大数据技术简介
数据规模到底多大才叫大数据？
大数据一词的重点不是在于数据规模的定义，而是指信息技术发展已经进入了一个新时代

大数据的特点：5V
- Volume: 大容量：PB级规模
- Varirty: 多样性：结构化/非结构化
- Velocity: 时效性： 实时处理
- Veracity： 准确性：结果准确
- Value： 大价值：深度价值

大数据处理技术产生的背景
- 多核/多处理器并行计算
- MapReduce大数据并行计算
- 行业大数据应用需求

大数据分析应用的ABCD关键要素
算法模型
业务场景
计算力
数据资源

结构特征
- 结构化数据
- 半结构化数据
  
获取和处理方式
- 静态（线下数据）/非实时数据
- 动态（流式/增量式/线上）/实时数据
  
关联特征
- 无关联/简单关联数据（键值记录型数据）
- 复杂关联数据（图数据）
  
大数据处理的主要技术问题

存储：巨量数据如何存得下

计算：巨量数据如何快速完成计算

分析：如何返现大数据的深度价值


大数据研究的基本途径

- 降低尺度，寻找数据尺度无关近似算法
- 新算法，寻找新算法降低计算复杂度
- 并行化，分而治之，并行化处理

## ch2 MapReduce简介

提高计算机硬件性能的而主要手段
- 提高字长、流水式微体系结构技术
- 提高集成度
- 提高主频

按照系统类型来分，并行计算系统通常包括
- 多核/众核、对称多处理、大规模并行处理、集群、网格等不同类型

按照程序设计模型/方法分类，并行计算主要可以分为
- 共享内存变量程序设计方式
- 消息传递程序设计方式
- MapReduce程序设计方式

为什么需要大规模数据并行处理
- 处理数据的能力大幅落后于数据增长
- 海量数据隐含着更准确的事实
  
什么是MapReduce
- 基于集群的高性能并行计算平台
- 并行程序开发与运行框架
- 并行程序设计模型与方法

为什么MapReduce如此重要？
- 高效的大规模数据处理方法
- 改变了大规模尺度上组织计算的方式
- 第一个不同于冯洛依曼结构的、基于集群而非单机的计算方式的重大突破
- 目前为止最为成功的基于大规模计算资源的并行计算抽象方法

对付大数据处理——分而治之
- 大数据分而治之的并行化计算
- 大数据任务划分和并行计算模型

构建抽象模型——Map和Reduce
- 主要设计思想：为大数据处理过程中的两个主要处理操作提供一种抽象机制
- 典型的流式大数据问题的特征
- Map和Reduce操作的抽象描述，提供一种抽象机制，把做什么和怎么做分开，程序员仅需要描述做什么，不需要关心怎么做
- 基于Map和Reduce的并行计算模型和计算过程

上升到框架——自动并行化并隐藏底层细节
- 主要需求、目标和设计思想
  - 实现自动并行化计算
  - 为程序员隐藏系统层细节
- MapReduce提供统一的构建并完成以下的主要功能
  - 任务调度
  - 数据/代码互定位
  - 出错处理
  - 分布式数据存储与文件管理
  - Comibiner和Partitioner(设计目的和作用)

MapReduce的主要设计思想和特征
- 向外横向扩展，而非向上纵向扩展
- 失效被认为是常态
- 吧计算处理向数据迁移
- 顺序处理数据、避免随机访问数据
- 为应用开发者隐藏系统层细节
- 平滑无缝的可扩展性
 
 MapReduce提供的主要功能
 - 任务调度：负责为划分后的就hi算任务分配和调度计算节点；同时负责监控这些节点的执行状态
 - 数据/代码互定位：为了减少数据通信，一个基本原则是本地化数据处理，即一个计算节点尽可能处理其本地磁盘上所分布存储的数据
 - 出错处理
 - 分布式数据存储与文件管理
 - Combiner和Partitioner： 将中间结果数据进入reduce节点前需要进行合并处理，把具有同样主键的数据合并到一起避免重复传送，优化网络传输，在Map之后执行；而Partitioner的主要设计目的和作用是数据分区，将数据分配到合适的Reduce节点，避免不同Reduce节点上数据的相关性，在Reduce之前执行

## ch3. Google/Hadoop MaqReduce基本构架

Google MapReduce的基本工作原理
- 并行化处理的基本过程，系统中有一个负责调度的主节点，以及数据Map和Reduce工作节点（Worker）
- 带宽优化（Combiner的设计目的和作用），优化到Reduce的结果
- 用数据分区解决数据相关性问题（Partitioner的设计目的和作用），即根据一定的策略对Map输出的中间结果进行分区
- 失效处理： 主节点失效checkpoinr,检查整个计算作业的执行情况；工作节点失效，直接终止
- 计算优化，把一个计算任务让多个Map节点同时做，取最快完成者的计算结果。冗余节点

分布式文件系统GFS的基本工作原理
- Google GFS的基本设计部原则
  - 廉价本地磁盘分布存储
  - 多数据自动备份解决可靠性
  - 为上层的MapReduce计算框架提供支撑

Google GFS的基本构架和工作原理
- GFS Master的主要作用
- GFS ChuckServer的主要作用
- 数据访问工作过程
- GFS的系统管理技术

分布式结构化数据表BigTable
- BigTable设计动机和目标
  - 需要存储管理海量的结构化半结构化数据
  - 海量的服务请求
  - 商用数据库无法使用
- BigTable数据模型——多维表
  - 行关键字、列关键字、时间戳

- BigTable基本构架
  - 子表服务器
  - 子表存储结构SSTable（对应于GFS数据块）
  - 子表数据格式
  - 子表寻址

Hadoop MapReduce主要组件
NameNode相当于Google的MasterServer
DataNode--ChunckServer
- 文件输入格式InputFormat <br>
定义文件如何分割和读取，选择文件或者其它让对象，用来作为输入；定义InputSplits，将一个文件分为不同任务，mapred.min.split.size；为RecordReader提供一个工厂，用来读取这个文件。
  - TextInputFormat, LineRecordReader
  - KeyValueTextInputFormat
  - SequenceFileInputFormat 
一个MapReduce程序统称为一个Job，一个Job有许多个任务构成
JobConf.setInputFormat

- Mapper <br>
每一个Mapper类的实例生成了一个Java进程，负责处理某一个InputSplit上的数据，用Maper.Context

- Combiner <br>
conf.setCombinerClass(Reduce.class)

- Partitioner && Shuffle
- Sort， 传输到每一个Reduce节点上的，将被所有的Reduce函数接收的Key,value会被hadoop自动排序
- Reducer
- OutputFormat <br>
与InputFormat对应

Hadoop系统中，JobTracker的主要作用是 作业控制（作业分解、状态监控），资源管理，而TaskTracker的主要作用是 汇报心跳、执行JobTracker的命令，NameNode的作用是存储了所有文件元数据、文件与数据块的映射关系，以及文件属性等核心数据；DataNode的作用是存储具体的数据快
## ch4 程序安装
## ch5 MapReduce算法设计
MapReduce处理流程
1. map
2. shuffle and sort
3. reduce

可编程控制部分
Mapper： setup(), map(), cleanup()
Shuffle: Partitioner(划分均匀，查找快速)
Sort
Reduce: setup(), reduce(), cleanup()

应用

1. 构建单词同现矩阵
2. 文档倒排索引，在map阶段单词在前，文档名在后，有效负载（如词频，只是需要在前一步进行统计即可）
3. 专利文献数据分析,统计被引文献，citing, cited

## ch6 HBase Hive

Hadoop HBase基本数据类型是一张多维表，该表的存储和检索有行关键字和列关键字、时间戳三个关键字组成，其数据操作访问是 通过JavaAPI和MapReduce接口编程实现，而Hive是一个分布式数据仓库，其数据操作访问编程是通过SQL实现

## ch7 高级MapReduce编程技术

用户自定义Partitioner和Combiner
组合式

## ch8 基于MapReduce的搜索引擎算法

PageRank算法

## ch9 数据挖掘基础算法

### k-means算法
将给定的多个对象分成若干组，组内的各个对象是相似的，组间的对象是不相似的。
数据点的类型可以划分为欧氏和非欧
对于欧式空间，取各个数据点的平均值
对于非欧空间，取某个处于最中间的店，取若干个最具代表性的点

k-means:
第一步，选区k个初始点,作为出事的cluster center
第二步，Loop {
    对输入中的每一个点p：
    {
        计算p到各个cluster的距离
        将p归入最近的cluster
    }
    重新计算各个cluster的中心
}
第三步，根据最终胜出的簇中心对所有数据元素进行划分聚类的工作


问题：
样本数据有n个，预期生产k个cluster，则k-means算法t次迭代过程中的时间复杂度为O(nkt)

并行化算法设计
思路： 在处理每一个数据点是，只需要知道cluster center的信息

将所有数据分布到不同的MapReduce节点上，每个节点只对自己的数据进行计算
每个Map节点能够读取上一次迭代生成的cluster center
Reduce节点综合每个属于每个cluster的数据点，计算出新的cluster centers (Reducer的个数实际上就和聚类中心的个数相同)

全局文件
- 当前的迭代次数
- K个不同聚类中心的数据结构（id, center, 数据点的个数）

    class Mapper
    setup(){
        读取全局的聚类中心数据 centers
    }
    map(key, p) //p 为一个数据点
    {
        minDis = Double.MAX_VALUE
        index = -1
        for i=0 to Centers.length
        {
            dis = ComputeDist(p, Centers[i])
            if (dis < minDis>)  // 替换k个点
            {
                minDis = dis;
                index = i;
            }
        }
        emit(Centers[i].ClusterID, (p,1)) 
        // 1表示数据点的个数
    }

    class Reducer
    reduce(ClusterID, value=[p(pm1, n1), (pm2, n2), ..])
    {
        pm = 0.0, n=0;
        k = 数据点列表中数据项的总个数
        for i=0 to k
        {
            pm += pm[i]*n[i]; n+=n[i];
        }
        pm = pm/n;
        emit(ClusterID, (pm,n));// 输出新的聚类中心的数据值
    }

### 基于MapReduce的分类并行化算法
从一组已经带有分类标记的训练样本数据集来预测一个测试样本的分类结果

#### kNN
k-NN: 计算测试样本到各训练样本的距离，取其中距离最小的K个，并根据这K个训练样本的标记进行投票得到测试样本的标记。

并行化思路
全局变量：
样本数据和k个最近的样本

对于每一个的样本数据，检查如果就hi算出来的值比目前大则替换，否则保留
根据所保留的k个最大的值得到最终的分类标记


#### 朴素贝叶斯分类

关键：对于一个未知类别的样本X，可以先分别计算出X属于每一个类别Yi的概率P(X|Yi)P(Yi), 然后选择其中概率最大的Yi作为其类别
理解：因为分母都是一样的，所以实际上考虑的就是分子的情况

并行化算法设计思路：
用MapReduce扫描数据集，计算每个类别Yi出现的频度FYi，以及每个属性值出现在Yi的频度FxYij

训练集的并行化代码
    class Mapper
    map(key, tr)
    {
        tr: trid,A,y
        emit(y,1) // 类别
        for i=0 to A.length
        {
            emit(<y, xni, xvi>, 1)
        }
    }
    class Reducer
    reduce(key, value_lsit) // Key为分类标记y, 或者<y, xni, xvi>
    {
        sum = 0
        while(value_lsit.hasNext()) {
            sum += value_lsit.next().get()
        }
        emit(key, sum)
    }

测试样本分类预测Mapper

    class Mapper
    setup()
    {
        读取从训练数据集中得到的频度数据
        分类频度表 FY = {(Yi, 每个Yi的频度FYi)}
        属性频度表 FxY = {(<Yi, xnj, xvj>, 出现频度FxYij)}}
    }
    map(key, ts) // ts为一个测试样本
    {
        ts tsid, A
        MaxF = MIN_VALUE; idx = -1
        扫描每个表，找到类别最大的表所在的位置
        emit(tsid, FY[idx].Yi)
    }

#### SVM短文本分类

训练阶段，对于多类（480类）问题，为了提高多类精度，首先针对每个类做一个2class两类分类器
预测阶段，分别用480个分类其对每个待预测的样本进行分类并打分，选择分类为”是“且打分最高的类别作为该样本可能的预测类别，则将该测试样本判定为不属于480类的异类

第一步，用训练数据产生480个2-class分类器模型
Map: 将每个训练样本的分类标签ClassID作为主键，输出（ClassID,  <true/ false, 特征向量>
Reduce： 具有相同ClassID的键值对进入同一个Reduce

第二步，用480个2-Class分类器模型处理测试数据
Map： 将每个测试样本，以SampleID作为主键，输出（SampleID, <LableID, 评分Score>）
Reduce: SampleID


## ch10 Spark系统及其编程技术

### Scala

1. val, var 不变引用和可变引用
2. 基于JVM、面向对象、函数式编程
3. 类是对象的抽象，对象是类的具体实现，占内存

### Spark vs Hadoop
Spark不足：
不擅长做以下计算：
1. 实时计算：Map Reduce无法在毫秒内或者秒级内返回结果
2. 流式计算：流式计算的输入数据是动态的，设计特点决定了数据源必须是静态的
3. DAG计算：多个应用程序存在依赖关系，后一个应用程序的输入为前一个的输出。每个MapReduce的阿左也输出结果都会写道磁盘，会造成大量的磁盘IO，性能非常的地下。
4. 没有利用好内存资源，频繁第磁盘草坪做
Spark优点：
1. Spark 把中间数据放到内存，迭代运算效率高。Spark支持DAG图的分布式并行计算的编程框架，减少了迭代过程中的数据的读写磁盘，提高了处理效率。（延迟加载）
2. 容错性高。Spark引进了弹性分布式数据集RDD的抽象，他是分布在一组节点的制度对象集合，这些集合式弹性的，如果数据集一部分丢失，则可以通过“血统：对它们进行重建。另外在RDD计算时可以通过CheckPoint来实现容错。
3. 更加通用。mapreduce只提供了map和reduce两种操作，而spark提供了很多种，大致分为Transformation和Action两大类

### RDDs
1. 基于RDD之间的依赖关系组成lineage,重计算以及checkpoint等机制保证容错性
2. 只读、可分区，这个数据集的全部或部分可以缓存在内存中，在多次计算间重用，弹性指内存不够时可以与磁盘进行交换

### Spark的基本构架和组件
Spark： mapreduce，rdd，function programing
本地运行模式，独立运行模式，mesos，yarn两套资源管理框架

Spark集群的基本结构
- Master node： 集群部署式的概念，式整个集群的控制器，负责整个集群的正常运行，管理Worker node
- Worker node： 式计算节点，接收主节点命令与进行状态汇报
- Executors: 每个Worker上有一个Executor，负责完成Task程序的执行
- Spark集群部署后，需要在主从节点启动Master进程和Worker进程，对整个集群进行控制


