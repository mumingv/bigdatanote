# Hadoop简介

## 为什么写《Hadoop实战》

略。


## 什么是Hadoop

Hadoop是一个开源的框架，可以编写和运行分布式应用处理大规模数据。

Hadoop集群是在同一地点用网络互联的一组通用机器。


## 了解分布式系统和Hadoop

一般意义上，系统有两种扩展方式：向上扩展和向外扩展。向上扩展是指升级单台服务器的处理能力；向外扩展是指使用更多的机器处理同一个任务，例如：分布式系统。

对于一个特别大的数据集（比如：4G大小的文件），Hadoop会先将数据集分割成较小的块（默认为64MB），然后将各块数据通过分布式文件系统（HDFS）分布在集群内的多台机器上。集群可以并行地读取数据，从而提供很高的吞吐量。

Hadoop强调把代码向数据迁移，而不是相反。这种方式符合Hadoop面向数据密集型处理的设计目标，因为代码和数据相比要小好几个数量级，移动起来更方便。


## 比较SQL数据库和Hadoop

SQL（结构化查询语言）是针对结构化数据设计的，而Hadoop的许多应用都是针对文本这种非结构化数据。

几点比较（Hadoop VS SQL）：
1. 用向外扩展代替向上扩展；
2. 用键值对代替关系表；
3. 用函数式编程（MapReduce）代替声明式查询（SQL）；
4. 用离线批量处理代替在线处理；


## 理解MapReduce

管道有助于进程原语的同步，已有模块的简单链接即可组成一个新的模块；消息队列则有助于进程原语的同步。程序员将数据处理任务以生产者或消费者的形式编写为进程原语，由系统来管理它们何时执行。

MapReduce是一个数据处理模型，其中的数据处理原语称为`mapper`和`reducer`。使用MapReduce写的程序，仅仅通过修改配置就可以将其扩展到集群中的其他机器上运行。


### 动手扩展一个简单程序

略。


### 相同程序在MapReduce中的扩展

略。


## 运行第一个程序（用Hadoop统计单词）

示例：[用Hadoop统计单词（单机模式）](#docs/hia_wordcount_standalone)。


## Hadoop历史

Hadoop的作者是Doug Cutting，他还有另外两个项目：Lucene和Nutch。

Lucene是一个功能全面的文本索引和查询库；Nutch是基于Lucene的一个完整的Web搜索引擎，是Lucene的一个子项目。

Hadoop起初是Nutch的一个子项目，后来流行开了之后，就升级为Apache的顶级项目了。

Hadoop基于Google的两篇论文实现，分别是：
1. GFS（Google文件系统）；
2. MapReduce框架。


## 小结

略。


## 参考资料

1. [Hadoop官方网站](http://hadoop.apache.org/)
2. [The Google File System](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/gfs-sosp2003.pdf)
3. [The Google File System 中文版](http://blog.bizcloudsoft.com/wp-content/uploads/Google-File-System%E4%B8%AD%E6%96%87%E7%89%88_1.0.pdf)
4. [MapReduce Simplified Data Processing on Large Clusters](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/mapreduce-osdi04.pdf)
5. [MapReduce Simplified Data Processing on Large Clusters 中文版](http://blog.bizcloudsoft.com/wp-content/uploads/Google-MapReduce%E4%B8%AD%E6%96%87%E7%89%88_1.0.pdf)
6. [Bigtable: A Distributed Storage System for Structured Data](http://static.googleusercontent.com/media/research.google.com/zh-CN//archive/bigtable-osdi06.pdf)
7. [Bigtable: A Distributed Storage System for Structured Data中文版](http://blog.bizcloudsoft.com/wp-content/uploads/Google-Bigtable%E4%B8%AD%E6%96%87%E7%89%88_1.0.pdf)
8. [使用命令行编译打包运行自己的MapReduce程序](http://www.powerxing.com/hadoop-build-project-by-shell/)

