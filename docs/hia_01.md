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

这里要运行一个简单的Hadoop统计单词程序，需要先安装JDK和Hadoop，安装步骤请参考：
1. [安装JDK](#docs/install#安装Java)
2. [安装Hadoop](#docs/install#安装Hadoop)

**说明**：这里的Hadoop只使用了一台机器，即：非分布式Hadoop，仅仅用于执行一些简单的Demo程序。

### 统计单词处理步骤

**说明**：下面的命令均使用`hadoop`用户进行操作。

#### 准备待统计的文件

创建`input`目录

```bash
[hadoop@CentOS ~]$ cd /usr/local/hadoop/
[hadoop@CentOS hadoop]$ mkdir input
```

将待统计文件放进`input`目录，比如，名称为`sample.txt`的文件内容如下：

```bash
[hadoop@CentOS input]$ cat sample.txt 
Do as I say, not as I do.
```

<font color="red">**重要说明**：`/usr/local/hadoop/`目录下不能有`output`目录，因为Hadoop在执行过程中会动态创建该目录，如果已经存在了，Hadoop程序就会执行失败。</font>

[可选]如果存在`output`目录，则在运行Hadoop程序执行，需要先将其删除或者改名。

```bash
[hadoop@CentOS hadoop]$ rm -rf output
```

#### 运行Hadoop自带的统计单词的mapreduce程序`wordcount`

程序执行的具体情况如下：

```bash
[hadoop@CentOS hadoop]$ ./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.2.jar wordcount input output
16/11/17 17:05:58 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/11/17 17:05:58 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
16/11/17 17:05:59 INFO input.FileInputFormat: Total input paths to process : 1
16/11/17 17:05:59 INFO mapreduce.JobSubmitter: number of splits:1
16/11/17 17:05:59 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local1677532496_0001
16/11/17 17:05:59 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/staging/hadoop1677532496/.staging/job_local1677532496_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/17 17:05:59 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/staging/hadoop1677532496/.staging/job_local1677532496_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/17 17:06:00 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/local/localRunner/hadoop/job_local1677532496_0001/job_local1677532496_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/17 17:06:00 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/local/localRunner/hadoop/job_local1677532496_0001/job_local1677532496_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/17 17:06:00 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
16/11/17 17:06:00 INFO mapreduce.Job: Running job: job_local1677532496_0001
16/11/17 17:06:00 INFO mapred.LocalJobRunner: OutputCommitter set in config null
16/11/17 17:06:00 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Waiting for map tasks
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Starting task: attempt_local1677532496_0001_m_000000_0
16/11/17 17:06:00 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/17 17:06:00 INFO mapred.MapTask: Processing split: file:/usr/local/hadoop/input/sample.txt:0+26
16/11/17 17:06:00 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/11/17 17:06:00 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/11/17 17:06:00 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/11/17 17:06:00 INFO mapred.MapTask: soft limit at 83886080
16/11/17 17:06:00 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/11/17 17:06:00 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/11/17 17:06:00 INFO mapred.LocalJobRunner: 
16/11/17 17:06:00 INFO mapred.MapTask: Starting flush of map output
16/11/17 17:06:00 INFO mapred.MapTask: Spilling map output
16/11/17 17:06:00 INFO mapred.MapTask: bufstart = 0; bufend = 58; bufvoid = 104857600
16/11/17 17:06:00 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214368(104857472); length = 29/6553600
16/11/17 17:06:00 INFO mapred.MapTask: Finished spill 0
16/11/17 17:06:00 INFO mapred.Task: Task:attempt_local1677532496_0001_m_000000_0 is done. And is in the process of committing
16/11/17 17:06:00 INFO mapred.LocalJobRunner: map
16/11/17 17:06:00 INFO mapred.Task: Task 'attempt_local1677532496_0001_m_000000_0' done.
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Finishing task: attempt_local1677532496_0001_m_000000_0
16/11/17 17:06:00 INFO mapred.LocalJobRunner: map task executor complete.
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Waiting for reduce tasks
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Starting task: attempt_local1677532496_0001_r_000000_0
16/11/17 17:06:00 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/17 17:06:00 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@567c66cc
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=363285696, maxSingleShuffleLimit=90821424, mergeThreshold=239768576, ioSortFactor=10, memToMemMergeOutputsThreshold=10
16/11/17 17:06:00 INFO reduce.EventFetcher: attempt_local1677532496_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
16/11/17 17:06:00 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local1677532496_0001_m_000000_0 decomp: 59 len: 63 to MEMORY
16/11/17 17:06:00 INFO reduce.InMemoryMapOutput: Read 59 bytes from map-output for attempt_local1677532496_0001_m_000000_0
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 59, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->59
16/11/17 17:06:00 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
16/11/17 17:06:00 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: finalMerge called with 1 in-memory map-outputs and 0 on-disk map-outputs
16/11/17 17:06:00 INFO mapred.Merger: Merging 1 sorted segments
16/11/17 17:06:00 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 54 bytes
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: Merged 1 segments, 59 bytes to disk to satisfy reduce memory limit
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: Merging 1 files, 63 bytes from disk
16/11/17 17:06:00 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
16/11/17 17:06:00 INFO mapred.Merger: Merging 1 sorted segments
16/11/17 17:06:00 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 54 bytes
16/11/17 17:06:00 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/17 17:06:00 INFO Configuration.deprecation: mapred.skip.on is deprecated. Instead, use mapreduce.job.skiprecords
16/11/17 17:06:00 INFO mapred.Task: Task:attempt_local1677532496_0001_r_000000_0 is done. And is in the process of committing
16/11/17 17:06:00 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/17 17:06:00 INFO mapred.Task: Task attempt_local1677532496_0001_r_000000_0 is allowed to commit now
16/11/17 17:06:00 INFO output.FileOutputCommitter: Saved output of task 'attempt_local1677532496_0001_r_000000_0' to file:/usr/local/hadoop/output/_temporary/0/task_local1677532496_0001_r_000000
16/11/17 17:06:00 INFO mapred.LocalJobRunner: reduce > reduce
16/11/17 17:06:00 INFO mapred.Task: Task 'attempt_local1677532496_0001_r_000000_0' done.
16/11/17 17:06:00 INFO mapred.LocalJobRunner: Finishing task: attempt_local1677532496_0001_r_000000_0
16/11/17 17:06:00 INFO mapred.LocalJobRunner: reduce task executor complete.
16/11/17 17:06:01 INFO mapreduce.Job: Job job_local1677532496_0001 running in uber mode : false
16/11/17 17:06:01 INFO mapreduce.Job:  map 100% reduce 100%
16/11/17 17:06:01 INFO mapreduce.Job: Job job_local1677532496_0001 completed successfully
16/11/17 17:06:01 INFO mapreduce.Job: Counters: 33
        File System Counters
                FILE: Number of bytes read=541172
                FILE: Number of bytes written=1009396
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
        Map-Reduce Framework
                Map input records=1
                Map output records=8
                Map output bytes=58
                Map output materialized bytes=63
                Input split bytes=104
                Combine input records=8
                Combine output records=6
                Reduce input groups=6
                Reduce shuffle bytes=63
                Reduce input records=6
                Reduce output records=6
                Spilled Records=12
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=68
                CPU time spent (ms)=0
                Physical memory (bytes) snapshot=0
                Virtual memory (bytes) snapshot=0
                Total committed heap usage (bytes)=268181504
        Shuffle Errors
                BAD_ID=0
                CONNECTION=0
                IO_ERROR=0
                WRONG_LENGTH=0
                WRONG_MAP=0
                WRONG_REDUCE=0
        File Input Format Counters 
                Bytes Read=26
        File Output Format Counters 
                Bytes Written=45
```

查看统计结果：

```bash
[hadoop@CentOS hadoop]$ cat output/part-r-00000 
Do      1
I       2
as      2
do.     1
not     1
say,    1
```

**说明**：
1. 生成的结果存放在名称为`output/part-***-*****`的文件中，本示例由于是单机Hadoop环境且文件特别小，所以只有生成了一个文件；如果文件较大（如：大于64M）被切分的话，则会生成多个文件，文件名称按照顺序进行编号；
2. Hadoop自带的单词统计mapreduce程序，默认是根据空格做的切词，并且单词的认定是区分大小写的，所以看到的是上面的结果。


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


