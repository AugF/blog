---
title: nju-course mapreduce experiment
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-09-19 12:07:54
tags:
- nju
- course
categories:
- [course, nju]
---
实验说明

## MapReduce Java编程

主要工作：
### 1 实现Map类
https://hadoop.apache.org/docs/r2.7.4/api/org/apache/hadoop/mapreduce/Mapper.html
Mapper是hadoop提供的抽象类  
Mapper<KEYIN,VALUEIN,KEYOUT,VALUEOUT>
- setup(Context context)
- map(KEYIN key,VALUEIN value, CONTEXT context)
- cleanup(Context context)
- run(CONTEXT context) // 一般不使用

KEYIN，VALUEIN...类型
>LongWritable -- long <br/>
>IntWritable -- int <br/>
>Text -- string <br/>
>Object -- void

> 这里往往还包含 Partitioner（Hash的方法）和Sort两个部分，一般编程不会涉及到
### 2 实现Reduce类
### 3 实现main函数 （Job）
conf -\> Job 见例子

[github hadoop example例子](https://github.com/apache/hadoop/tree/trunk/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples)
## ch2 wordcount

### hdfs 相关命令

hadoop fs:
- -ls -R
- -rmr
- -put localfile dfsfilepath
> eg: -put data/wordcount/*.html input/wordcount <br/>
> hdfs://绝对路径
- -cat file.data
- -mkdir -p 
- -get 

hadoop jar localjar_path class_path para1 para2

### paras
> 注意这里的路径只能hdfs上的文件
- inputPath: must exisit, file or dir
- outputPath: dir, the dir can't exisit

## ch3 带词频的倒排索引
[InvertedIndex.java](https://www.jianshu.com/p/5e059dbad553)

具体设计跟之前的一样

### paras
- inputPath: dir
- outputPath

scp命令使用
> scp -r target/Lab-1.0-SNAPSHOT.jar eugenewang@114.212.81.14:~/Documents

 hadoop jar /home/eugenewang/Documents/Lab-1.0-SNAPSHOT.jar InvertedIndex /input/invertedindex/exp3_sample_data /output/invertedindex/exp3

## 其他知识

关于FileSplit,InputSplit更多介绍
[FileInputFormat类中split切分算法和host选择算法介绍](https://blog.csdn.net/xingliang_li/article/details/53285447?utm_source=blogxgwz4)

[Class FileSplit](https://hadoop.apache.org/docs/r2.9.0/api/org/apache/hadoop/mapred/FileSplit.html)

## 扩展
1. 使用另外一个 MapReduce Job 对每个词语的平均出现次数进行全局排序,
输出排序后的结果。

思考：新建一个排序job,将临时文件作为输出，在其map中，我们将输入每一行进行分割，将词频作为key，其他的作为value,重载Comparator类，进行从小到大排序，之后进行输出即可
