# 用Hadoop统计单词（伪分布模式）

## 前提

请务必确保如下操作均已完成：
1. [安装JDK](#docs/install#安装JDK)
2. [安装Hadoop单机模式](#docs/install#单机模式安装步骤)
3. [安装Hadoop伪分布模式](#docs/install#伪分布模式安装步骤)


## 步骤

*如未作特别说明，所有的命令均使用hadoop用户在目录/user/local/hadoop目录下执行。*

### 创建用户主目录

在HDFS中创建用户名为hadoop的主目录（类似Linux文件系统中的/home/hadoop目录）

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -mkdir -p /user/hadoop
```

<font color="red">重要说明：对于单机模式，其所使用的文件系统就是Linux系统自带的；而对于伪分布模式和全分布模式，其所使用的文件系统就是HDFS了，要操作HDFS里面的文件就需要使用Hadoop提供的命令来操作。</font>

*可以使用如下命令查看已经创建好的目录*

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -ls -R /
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:06 /user
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:13 /user/hadoop
```

*说明：`hdfs dfs`命令也可以用`hadoop fs`命令替换，两者功能完全一致。*

```bash
[hadoop@CentOS hadoop]$ ./bin/hadoop fs -ls -R /             
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:06 /user
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:13 /user/hadoop
```


### 准备待统计的文件

#### 创建input目录

使用`hdfs dfs -mkdir`命令在HDFS中创建`input`目录.

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -mkdir input
```

*同样地，我们查询一下创建的目录。*

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -ls -R /    
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:06 /user
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:13 /user/hadoop
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:12 /user/hadoop/input
```

*当然，你也可以使用下面的方式查看。*

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -ls
Found 1 items
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:12 input
```

*这种方式就类似在/user/hadoop目录下执行`ls`命令一样，这也从侧面反映了HDFS的默认工作目录就是我们在开始创建的/user/hadoop目录。*


#### 上传文件

这里还是使用单机模式是一的统计文件，文件内容如下：

```bash
[hadoop@CentOS hadoop]$ cat input/sample.txt 
Do as I say, not as I do.
```

使用`hdfs dfs -put`命令将本地文件`input/sample.txt`上传到HDFS的`input`目录中。

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -put input/sample.txt input/
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -ls -R /
drwxr-xr-x   - hadoop supergroup          0 2016-11-19 21:06 /user
drwxr-xr-x   - hadoop supergroup          0 2016-11-20 11:40 /user/hadoop
drwxr-xr-x   - hadoop supergroup          0 2016-11-20 11:41 /user/hadoop/input
-rw-r--r--   1 hadoop supergroup         26 2016-11-20 11:41 /user/hadoop/input/sample.txt
```


### 运行Hadoop自带的统计单词的mapreduce程序`wordcount`

程序执行的具体情况如下：

```bash
[hadoop@CentOS hadoop]$ ./bin/hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.2.jar wordcount input output
16/11/20 11:47:45 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/11/20 11:47:45 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
16/11/20 11:47:46 INFO input.FileInputFormat: Total input paths to process : 1
16/11/20 11:47:46 INFO mapreduce.JobSubmitter: number of splits:1
16/11/20 11:47:46 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local701433384_0001
16/11/20 11:47:46 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/staging/hadoop701433384/.staging/job_local701433384_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/20 11:47:46 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/staging/hadoop701433384/.staging/job_local701433384_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/20 11:47:47 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/local/localRunner/hadoop/job_local701433384_0001/job_local701433384_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/11/20 11:47:47 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/local/localRunner/hadoop/job_local701433384_0001/job_local701433384_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/11/20 11:47:47 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
16/11/20 11:47:47 INFO mapreduce.Job: Running job: job_local701433384_0001
16/11/20 11:47:47 INFO mapred.LocalJobRunner: OutputCommitter set in config null
16/11/20 11:47:47 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapreduce.lib.output.FileOutputCommitter
16/11/20 11:47:47 INFO mapred.LocalJobRunner: Waiting for map tasks
16/11/20 11:47:47 INFO mapred.LocalJobRunner: Starting task: attempt_local701433384_0001_m_000000_0
16/11/20 11:47:47 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/20 11:47:47 INFO mapred.MapTask: Processing split: hdfs://localhost:9000/user/hadoop/input/sample.txt:0+26
16/11/20 11:47:47 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/11/20 11:47:47 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/11/20 11:47:47 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/11/20 11:47:47 INFO mapred.MapTask: soft limit at 83886080
16/11/20 11:47:47 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/11/20 11:47:47 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/11/20 11:47:47 INFO mapred.LocalJobRunner: 
16/11/20 11:47:47 INFO mapred.MapTask: Starting flush of map output
16/11/20 11:47:47 INFO mapred.MapTask: Spilling map output
16/11/20 11:47:47 INFO mapred.MapTask: bufstart = 0; bufend = 58; bufvoid = 104857600
16/11/20 11:47:47 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214368(104857472); length = 29/6553600
16/11/20 11:47:47 INFO mapred.MapTask: Finished spill 0
16/11/20 11:47:47 INFO mapred.Task: Task:attempt_local701433384_0001_m_000000_0 is done. And is in the process of committing
16/11/20 11:47:47 INFO mapred.LocalJobRunner: map
16/11/20 11:47:47 INFO mapred.Task: Task 'attempt_local701433384_0001_m_000000_0' done.
16/11/20 11:47:47 INFO mapred.LocalJobRunner: Finishing task: attempt_local701433384_0001_m_000000_0
16/11/20 11:47:47 INFO mapred.LocalJobRunner: map task executor complete.
16/11/20 11:47:47 INFO mapred.LocalJobRunner: Waiting for reduce tasks
16/11/20 11:47:47 INFO mapred.LocalJobRunner: Starting task: attempt_local701433384_0001_r_000000_0
16/11/20 11:47:47 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/11/20 11:47:47 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@1f707a6a
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=363285696, maxSingleShuffleLimit=90821424, mergeThreshold=239768576, ioSortFactor=10, memToMemMergeOutputsThreshold=10
16/11/20 11:47:47 INFO reduce.EventFetcher: attempt_local701433384_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
16/11/20 11:47:47 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local701433384_0001_m_000000_0 decomp: 59 len: 63 to MEMORY
16/11/20 11:47:47 INFO reduce.InMemoryMapOutput: Read 59 bytes from map-output for attempt_local701433384_0001_m_000000_0
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 59, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->59
16/11/20 11:47:47 WARN io.ReadaheadPool: Failed readahead on ifile
EBADF: Bad file descriptor
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX.posix_fadvise(Native Method)
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX.posixFadviseIfPossible(NativeIO.java:263)
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX$CacheManipulator.posixFadviseIfPossible(NativeIO.java:142)
        at org.apache.hadoop.io.ReadaheadPool$ReadaheadRequestImpl.run(ReadaheadPool.java:206)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
16/11/20 11:47:47 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
16/11/20 11:47:47 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: finalMerge called with 1 in-memory map-outputs and 0 on-disk map-outputs
16/11/20 11:47:47 INFO mapred.Merger: Merging 1 sorted segments
16/11/20 11:47:47 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 54 bytes
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: Merged 1 segments, 59 bytes to disk to satisfy reduce memory limit
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: Merging 1 files, 63 bytes from disk
16/11/20 11:47:47 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
16/11/20 11:47:47 INFO mapred.Merger: Merging 1 sorted segments
16/11/20 11:47:47 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 54 bytes
16/11/20 11:47:47 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/20 11:47:47 INFO Configuration.deprecation: mapred.skip.on is deprecated. Instead, use mapreduce.job.skiprecords
16/11/20 11:47:48 INFO mapred.Task: Task:attempt_local701433384_0001_r_000000_0 is done. And is in the process of committing
16/11/20 11:47:48 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/11/20 11:47:48 INFO mapred.Task: Task attempt_local701433384_0001_r_000000_0 is allowed to commit now
16/11/20 11:47:48 INFO output.FileOutputCommitter: Saved output of task 'attempt_local701433384_0001_r_000000_0' to hdfs://localhost:9000/user/hadoop/output/_temporary/0/task_local701433384_0001_r_000000
16/11/20 11:47:48 INFO mapred.LocalJobRunner: reduce > reduce
16/11/20 11:47:48 INFO mapred.Task: Task 'attempt_local701433384_0001_r_000000_0' done.
16/11/20 11:47:48 INFO mapred.LocalJobRunner: Finishing task: attempt_local701433384_0001_r_000000_0
16/11/20 11:47:48 INFO mapred.LocalJobRunner: reduce task executor complete.
16/11/20 11:47:48 INFO mapreduce.Job: Job job_local701433384_0001 running in uber mode : false
16/11/20 11:47:48 INFO mapreduce.Job:  map 100% reduce 100%
16/11/20 11:47:48 INFO mapreduce.Job: Job job_local701433384_0001 completed successfully
16/11/20 11:47:48 INFO mapreduce.Job: Counters: 38
        File System Counters
                FILE: Number of bytes read=541148
                FILE: Number of bytes written=1011773
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=52
                HDFS: Number of bytes written=33
                HDFS: Number of read operations=15
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=4
        Map-Reduce Framework
                Map input records=1
                Map output records=8
                Map output bytes=58
                Map output materialized bytes=63
                Input split bytes=115
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
                GC time elapsed (ms)=55
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
                Bytes Written=33
```

查看统计结果：

```bash
[hadoop@CentOS hadoop]$ ./bin/hdfs dfs -cat output/*
Do      1
I       2
as      2
do.     1
not     1
say,    1
```

*可以看到这里执行mapreduce程序的方法和单机模式是完全一样的，区别仅仅在于读取文件的位置。单机模式的输入文件在Linux文件系统中，而伪分布模式的输入文件中HDFS分布式文件系统中。*

*可以使用`hdfs dfs -get`命令将HDFS中的文件取回到本地来。如：将伪分布模式下运行程序得到的结果`output`目录取回到本地并且重命名为hdfs_output。*

```bash
[hadoop@CentOS hadoop]$ hdfs dfs -get output ./hdfs_output
[hadoop@CentOS hadoop]$ cat hdfs_output/*
Do      1
I       2
as      2
do.     1
not     1
say,    1
```

*同单机模式一样，每次运行mapreduce时都需要确保HDFS当前工作目录下不能有output目录，否则程序会运行失败。删除HDFS的output目录的方法如下。*

```bash
[hadoop@CentOS hadoop]$ hdfs dfs -rm -r output
16/11/20 13:15:42 INFO fs.TrashPolicyDefault: Namenode trash configuration: Deletion interval = 0 minutes, Emptier interval = 0 minutes.
Deleted output
```

