---
title: spark study thinking
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 13:09:17
tags:
- spark
- thinking
categories:
- [spark, thinking]
---

spark中map就是把数据集分配到各个节点上，然后各个节点执行
> 思考Partioner的过程是什么

Partitioner感觉上是全局变量
> HashPartitioner就是把Hash桶做成了

inputSplit, 一定要比它大，这样就可以使得每一部分执行的数据少


flatMap并没有那么简单，也就是说
flatMap = map + flatten
> flatMap(t=>f(t))
f返回的是什么类型，最终结果就是什么类型


reduce(x+y=>x)
reduceByKey(x+y=>x)

```
mapPartitions(iter => {    // mapPartitions即 Mapper端的数据进行了Combine计数
      val pair = mutable.Map.empty[WrapArray, Long]
      while (iter.hasNext) {
        val arr = new WrapArray(iter.next())
        val value = pair.get(arr)
        if (value == None) {
          pair(arr) = 1L
        } else {
          pair(arr) = value.get + 1L
        }
      }
      pair.iterator   // 这种写法很精妙，进行学习
    })
    // 每次返回一个迭代器，下次还可以考虑继续使用这个迭代器
```


基本类型的迭代器可以定义

自定义迭代器
https://blog.csdn.net/XiaoHeiBlack/article/details/77014626


groupByKey(): RDD, Iterable[V]
reduceByKey(): U+U=>U

> 思考迭代器


FPGrowth思路整理

MLlib做法：
1. 产生每个分区的数据集
> val part = partitioner.getPartition(item), 这一步依然感觉很懵逼? 
> mapreduce中map的过程实际上就不过是map的数据集的个体变大的。所以按照自己想的就是 
> 代码这里的感觉就是用的默认集群的？？？
2. 将每个分区的数据集合并为一棵子树
3. 将所有分区合并为一个大的子树
4. 从这个大的子树中提取对应的频繁项集


defaultParallelism(): cluster default
trait: 相当于接口


sc.text(): 这里默认的也是这个参数



FPNewDef做法
1. 产生对应分区的数据集
2. 对每个分区进行相同合并
3. 数据重新写为(key, value)
4. 映射到对应的分区
> 所以就要用分区内的排序来做
repartitionAndSortWithinPartitions
https://www.jianshu.com/p/5906ddb5bfcd
5. 分区内根据顺序收集，从而形成条件树
> 这里可以直接根据收集结果来产生频繁项集啊

NewIdea
1. 产生分区的数据集
2. 可选，Combine(需要开销)
3. 数据重新分为(key, value)
4. reduceByKey
> 收集对应的key，然后直接产生条件树

> 因为类没有实现迭代器的特性，所以不能这样用？ 居然可以用排序就可以了，牛逼，牛逼
> 如果是对所有的元素做某种运算，但不需要产生最后的结果，只需要foreach

> 第一个想法，一个是将每个(key,value)的value映射为一个FPNewNode, 然后添加，需要重新改写数据结构
> 第二个想法，reduceByKey，先收集到一个集合，然后再对该集合进行下一步做法


? spark任务调用顺序

spark-shell --master spark://slave033:7077 

例子：
1 2 3 4
1 2 4
2 3
2 4

partion: 2

1:
 1 2 3 4  ->  
 1 2 4

2:
 2 3
 2 4

总结：
FPTree中的extract是对所有的项都要来直接求解，试着不需要。只需要对某一个地方进行求解即可。
然后这里的 extract又是个递归

summries中包含所有元素

> 怎么生成数据？关于大数据集的
> slave033上的文件和hdfs上的文件有什么区别？
>> 这个问题实际上是想问这里用到的是啥？

本地进行模拟运行
## 学到的知识

### 1. spark.default.parallelism
> 如果不进行设置，默认值是底层HDFS的block数量（HDFS中有设置）; 一般这是个特别需要设置； 在默认情况下，非常小，如果不调节它就起不到分布式的效果了。通常来说也不会只调节到num_executors*executor-cores，因为相当于只给每个节点分配了对应很少的值，所以实际上的话，是在2~3倍。为什么呢，因为有的task运行速度快，有的慢，为了避免快的执行完等慢的，所以可以考虑多加进行设置，这样的话就能充分利用到集群了。
> ? hdfs block的数量？？
https://www.iteblog.com/archives/1659.html


### 2. scala语法

```
class FPTree
  def merge(other: FPTree[T]): this.type = {
    other.transactions.foreach { case (t, c) =>
      add(t, c)  // 这里就默认的是this.add()
    }
    this
  }
```

### 3. Spark参数配置优先级
 1. SparkConf
 2. spark-submit  spark-shell
 3. spark-defaults.conf
> 在文件中进行配置
### 4. 零散知识点

#### 4.1 提交
1. spark-shell
-  --master spark://host:port本身的运行模式
- --deploy-mode: driver端位于哪里
- dirver-memory, dirver-cores
> driver-memory, driver-cores指的是driver端执行的情况 

2. 运行模式
- standalone模式
- spark on yarn, dirver在本地
- spark on yarn, dirver在集群
> 其实standalone和其他没有什么区别吧，主要就在于standalone是主模式，而yarn各个节点都相同

3. yarn

yarn: 整个资源管理在一个资源池中，想要获取资源通过在资源池中进行获取
> 这里yarn把整个集群集合在了一起，具体yarn是怎么执行的需要看居然的yarn的东西。

然后，关于yarn的就有几个特别的参数：

worker是资源分配单位
executor是任务执行单位

executor的内存主要分为几块
1. 执行自己的代码所需要的内存,默认值20%
2. shuffle所需要的内存，默认20%
3. 持久化的内存,默认60%

资源参数调优:
1. num-executors: 总的executor数
> 总共的任务执行单元

2. executor-cores 


### inputPath
> 在哪里？
hdfs://master:port/根路径

> 默认情况下为master节点上的9000端口


注意到一个问题： aggreateByKey一直都是根据key来处理
reduceByKey(partioner, _+_) 可以使结果放在对应的位置上

学长的代码，想法依然是那样的。感觉上还是最终落在了收集fpTree上，而且并没有什么优化啊。

前面汇聚相同的什么用?

在倒数第二个的时候依然是形成fpTree,然后提取


## scala编程
注意区分Iterator和


## 师兄代码的想法

建树开销，即考虑的是


spark-submit 默认参数在 spark-defaults.conf中
？ master是否进行资源的分配

--dirver-memory 
https://www.cnblogs.com/weiweifeng/p/8073553.html

