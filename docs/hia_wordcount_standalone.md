# 用Hadoop统计单词（单机/本地模式）

## 前提

请务必确保如下操作均已完成：
1. [安装JDK](#docs/install#安装JDK)
2. [安装Hadoop](#docs/install#单机模式安装步骤)

*说明：这里的Hadoop只使用了一台机器，即：单机模式Hadoop，仅仅用于执行一些简单的Demo程序。*

## 步骤

*说明：下面的命令均使用`hadoop`用户进行操作。*

### 准备待统计的文件

#### 创建input目录

```bash
[hadoop@CentOS ~]$ cd /usr/local/hadoop/
[hadoop@CentOS hadoop]$ mkdir input
```

#### 准备文件

将待统计文件放进`input`目录，比如，名称为`sample.txt`的文件内容如下：

```bash
[hadoop@CentOS input]$ cat sample.txt 
Do as I say, not as I do.
```

<font color="red">重要说明：`/usr/local/hadoop/`目录下不能有`output`目录，因为Hadoop在执行过程中会动态创建该目录，如果已经存在了，Hadoop程序就会执行失败。</font>

【可选】如果存在`output`目录，则在运行Hadoop的mapreduce程序之前，需要先将其删除或者改名。

```bash
[hadoop@CentOS hadoop]$ rm -rf output
```

### 运行Hadoop自带的统计单词的mapreduce程序wordcount

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

备注：
1. 生成的结果存放在名称为`output/part-***-*****`的文件中，本示例由于是单机Hadoop环境且文件特别小，所以只有生成了一个文件；如果文件较大（如：大于64M）被切分的话，则会生成多个文件，文件名称按照顺序进行编号；
2. Hadoop自带的单词统计mapreduce程序，默认是根据空格做的切词，并且单词的认定是区分大小写的，所以看到的是上面的结果。


## 更进一步（自行编译）

默认的单词统计程序有一些显而易见的问题，主要有如下几个：
1. 统计的结果中将标点符号也当作了单词的一部分；
2. 单词区分了大小写；
3. 不能控制低频词的输出，如：要求只出现一次的单词不输出。

我们需要针对默认的单词统计程序做一些修改来解决这几个问题。具体的处理步骤如下：

### 获取源码

安装后的Hadoop是不带程序源码的，需要自行下载：[Hadoop源码](http://mirrors.cnnic.cn/apache/hadoop/common/hadoop-2.5.2/hadoop-2.5.2-src.tar.gz)。

```bash
[hadoop@CentOS ~]$ cd usr/local/src/
[hadoop@CentOS src]$ ls
hadoop-2.5.2-src.tar.gz
```

解压：

```bash
[hadoop@CentOS src]$ tar xvzf hadoop-2.5.2-src.tar.gz
[hadoop@CentOS src]$ ls
hadoop-2.5.2-src  hadoop-2.5.2-src.tar.gz
```

单词统计程序源码的位置如下：

/home/hadoop/usr/local/src/hadoop-2.5.2-src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/WordCount.java

*注意：另外一个目录下也存在一个同名的单词统计程序，这个统计程序是hadoop 1.x版本的，不要取错。*

*/home/hadoop/usr/local/src/hadoop-2.5.2-src/hadoop-mapreduce-project/hadoop-mapreduce-client/hadoop-mapreduce-client-jobclient/src/test/java/org/apache/hadoop/mapred/WordCount.java*


### 修改代码

创建工作目录mycode，并将单词统计程序拷贝到mycode/src下。

```bash
[hadoop@CentOS ~]$ cd /usr/local/hadoop/
[hadoop@CentOS hadoop]$ mkdir -p mycode/src
[hadoop@CentOS hadoop]$ mkdir -p mycode/classes
[hadoop@CentOS hadoop]$ cp /home/hadoop/usr/local/src/hadoop-2.5.2-src/hadoop-mapreduce-project/hadoop-mapreduce-examples/src/main/java/org/apache/hadoop/examples/WordCount.java mycode/src/
```

修改后的代码请参考：[WordCount.java](https://github.com/mumingv/bigdata/blob/master/books/my_hadoop_in_action/mycode/src/WordCount.java)。

<font color="red">特别需要注意的是，一定要修改源码中的package路径，否则在运行阶段会发现自己编译的程序并不会被执行。造成此问题的原因是该package路径下存在Hadoop自带的同名WordCount程序。</font>

源码中的package路径是：
```java
package org.apache.hadoop.examples;
```
修改为任意其他的路径，比如：
```java
package muming.examples;
```


### 编译

```bash
[hadoop@CentOS hadoop]$ javac -d mycode/classes/ mycode/src/WordCount.java         
Note: mycode/src/WordCount.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
[hadoop@CentOS hadoop]$ ls mycode/classes/muming/examples/
WordCount.class  WordCount$IntSumReducer.class  WordCount$TokenizerMapper.class
```


### 打包

```bash
[hadoop@CentOS hadoop]$ jar -cvf mycode/WordCount.jar -C mycode/classes/ .          
added manifest
adding: muming/(in = 0) (out= 0)(stored 0%)
adding: muming/examples/(in = 0) (out= 0)(stored 0%)
adding: muming/examples/WordCount.class(in = 1933) (out= 1050)(deflated 45%)
adding: muming/examples/WordCount$IntSumReducer.class(in = 1782) (out= 762)(deflated 57%)
adding: muming/examples/WordCount$TokenizerMapper.class(in = 1873) (out= 814)(deflated 56%)
```

### 运行

```bash
[hadoop@CentOS hadoop]$ bin/hadoop jar mycode/WordCount.jar muming.examples.WordCount input output
16/11/18 16:46:05 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/11/18 16:46:05 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
16/11/18 16:46:06 INFO input.FileInputFormat: Total input paths to process : 1
16/11/18 16:46:06 INFO mapreduce.JobSubmitter: number of splits:1
16/11/18 16:46:06 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local1327785071_0001
16/11/18 16:46:06 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/staging/hadoop1327785071/.staging/job_local1327785071_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/18 16:46:06 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/staging/hadoop1327785071/.staging/job_local1327785071_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/18 16:46:07 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/local/localRunner/hadoop/job_local1327785071_0001/job_local1327785071_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/18 16:46:07 WARN conf.Configuration: file:/tmp/hadoop-hadoop/mapred/local/localRunner/hadoop/job_local1327785071_0001/job_local1327785071_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/18 16:46:07 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
16/11/18 16:46:07 INFO mapreduce.Job: Running job: job_local1327785071_0001
16/11/18 16:46:07 INFO mapred.LocalJobRunner: OutputCommitter set in config null
16/11/18 16:46:07 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Waiting for map tasks
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Starting task: attempt_local1327785071_0001_m_000000_0
16/11/18 16:46:07 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/18 16:46:07 INFO mapred.MapTask: Processing split: file:/usr/local/hadoop/input/sample.txt:0+26
16/11/18 16:46:07 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/11/18 16:46:07 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/11/18 16:46:07 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/11/18 16:46:07 INFO mapred.MapTask: soft limit at 83886080
16/11/18 16:46:07 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/11/18 16:46:07 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/11/18 16:46:07 INFO mapred.LocalJobRunner: 
16/11/18 16:46:07 INFO mapred.MapTask: Starting flush of map output
16/11/18 16:46:07 INFO mapred.MapTask: Spilling map output
16/11/18 16:46:07 INFO mapred.MapTask: bufstart = 0; bufend = 56; bufvoid = 104857600
16/11/18 16:46:07 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214368(104857472); length = 29/6553600
16/11/18 16:46:07 INFO mapred.MapTask: Finished spill 0
16/11/18 16:46:07 INFO mapred.Task: Task:attempt_local1327785071_0001_m_000000_0 is done. And is in the process of committing
16/11/18 16:46:07 INFO mapred.LocalJobRunner: map
16/11/18 16:46:07 INFO mapred.Task: Task 'attempt_local1327785071_0001_m_000000_0' done.
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Finishing task: attempt_local1327785071_0001_m_000000_0
16/11/18 16:46:07 INFO mapred.LocalJobRunner: map task executor complete.
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Waiting for reduce tasks
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Starting task: attempt_local1327785071_0001_r_000000_0
16/11/18 16:46:07 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/18 16:46:07 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@4869a267
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=363285696, maxSingleShuffleLimit=90821424, mergeThreshold=239768576, ioSortFactor=10, memToMemMergeOutputsThreshold=10
16/11/18 16:46:07 INFO reduce.EventFetcher: attempt_local1327785071_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
16/11/18 16:46:07 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local1327785071_0001_m_000000_0 decomp: 28 len: 32 to MEMORY
16/11/18 16:46:07 INFO reduce.InMemoryMapOutput: Read 28 bytes from map-output for attempt_local1327785071_0001_m_000000_0
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 28, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->28
16/11/18 16:46:07 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
16/11/18 16:46:07 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: finalMerge called with 1 in-memory map-outputs and 0 on-disk map-outputs
16/11/18 16:46:07 INFO mapred.Merger: Merging 1 sorted segments
16/11/18 16:46:07 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 23 bytes
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: Merged 1 segments, 28 bytes to disk to satisfy reduce memory limit
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: Merging 1 files, 32 bytes from disk
16/11/18 16:46:07 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
16/11/18 16:46:07 INFO mapred.Merger: Merging 1 sorted segments
16/11/18 16:46:07 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 23 bytes
16/11/18 16:46:07 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/18 16:46:07 INFO Configuration.deprecation: mapred.skip.on is deprecated. Instead, use mapreduce.job.skiprecords
16/11/18 16:46:07 INFO mapred.Task: Task:attempt_local1327785071_0001_r_000000_0 is done. And is in the process of committing
16/11/18 16:46:07 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/18 16:46:07 INFO mapred.Task: Task attempt_local1327785071_0001_r_000000_0 is allowed to commit now
16/11/18 16:46:07 INFO output.FileOutputCommitter: Saved output of task 'attempt_local1327785071_0001_r_000000_0' to file:/usr/local/hadoop/output/_temporary/0/task_local1327785071_0001_r_000000
16/11/18 16:46:07 INFO mapred.LocalJobRunner: reduce > reduce
16/11/18 16:46:07 INFO mapred.Task: Task 'attempt_local1327785071_0001_r_000000_0' done.
16/11/18 16:46:07 INFO mapred.LocalJobRunner: Finishing task: attempt_local1327785071_0001_r_000000_0
16/11/18 16:46:07 INFO mapred.LocalJobRunner: reduce task executor complete.
16/11/18 16:46:08 INFO mapreduce.Job: Job job_local1327785071_0001 running in uber mode : false
16/11/18 16:46:08 INFO mapreduce.Job:  map 100% reduce 100%
16/11/18 16:46:08 INFO mapreduce.Job: Job job_local1327785071_0001 completed successfully
16/11/18 16:46:08 INFO mapreduce.Job: Counters: 33
        File System Counters
                FILE: Number of bytes read=7842
                FILE: Number of bytes written=471724
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
        Map-Reduce Framework
                Map input records=1
                Map output records=8
                Map output bytes=56
                Map output materialized bytes=32
                Input split bytes=104
                Combine input records=8
                Combine output records=3
                Reduce input groups=3
                Reduce shuffle bytes=32
                Reduce input records=3
                Reduce output records=3
                Spilled Records=6
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=38
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
                Bytes Written=26
```

*说明：如果参数'WordCount'不加package路径的话，会报错下错误。*

```bash
[hadoop@CentOS hadoop]$ bin/hadoop jar mycode/WordCount.jar WordCount input output                
Exception in thread "main" java.lang.ClassNotFoundException: WordCount
        at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
        at java.lang.Class.forName0(Native Method)
        at java.lang.Class.forName(Class.java:278)
        at org.apache.hadoop.util.RunJar.main(RunJar.java:205)
```


### 查看结果

```bash
[hadoop@CentOS hadoop]$ cat output/*
as      2
do      2
i       2
```


### 参考资料

1. [使用命令行编译打包运行自己的MapReduce程序](http://www.powerxing.com/hadoop-build-project-by-shell/)
