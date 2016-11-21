# 用Hadoop统计单词（全分布/集群模式）

## 前提

请务必确保如下操作均已完成：
1. [安装JDK](#docs/install#安装JDK)
2. [安装Hadoop单机模式](#docs/install#单机模式安装步骤)
3. [安装Hadoop全分布模式](#docs/install#全分布模式安装步骤)


## 步骤

*如未作特别说明，所有的命令均在master节点上使用hadoop用户在/user/local/hadoop目录下执行。*

### 创建用户主目录

在HDFS中创建用户名为hadoop的主目录（类似Linux文件系统中的/home/hadoop目录）

```bash
[hadoop@CentOS hadoop]$ hdfs dfs -mkdir -p /user/hadoop
```

<font color="red">重要说明：对于单机模式，其所使用的文件系统就是Linux系统自带的；而对于伪分布模式和全分布模式，其所使用的文件系统就是HDFS了，要操作HDFS里面的文件就需要使用Hadoop提供的命令来操作。</font>

*可以使用如下命令查看已经创建好的目录*

```bash
[hadoop@master hadoop]$ hdfs dfs -mkdir -p /user/hadoop
[hadoop@master hadoop]$ hdfs dfs -ls -R /
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done
drwxrwxrwt   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done_intermediate
drwxr-xr-x   - hadoop supergroup   0 2016-11-21 14:11 /user
drwxr-xr-x   - hadoop supergroup   0 2016-11-21 14:11 /user/hadoop
```

*说明：`hdfs dfs`命令也可以用`hadoop fs`命令替换，两者功能完全一致。*

```bash
[hadoop@master hadoop]$ hadoop fs -ls -R /
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history
drwxrwx---   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done
drwxrwxrwt   - hadoop supergroup   0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done_intermediate
drwxr-xr-x   - hadoop supergroup   0 2016-11-21 14:11 /user
drwxr-xr-x   - hadoop supergroup   0 2016-11-21 14:11 /user/hadoop
```


### 准备待统计的文件

#### 创建input目录

使用`hdfs dfs -mkdir`命令在HDFS中创建`input`目录.

```bash
[hadoop@master hadoop]$ hdfs dfs -mkdir input
```

*同样地，我们查询一下创建的目录。*

```bash
[hadoop@master hadoop]$ hdfs dfs -ls -R /
drwxrwx---   - hadoop supergroup          0 2016-11-21 13:41 /tmp
drwxrwx---   - hadoop supergroup          0 2016-11-21 13:41 /tmp/hadoop-yarn
drwxrwx---   - hadoop supergroup          0 2016-11-21 13:41 /tmp/hadoop-yarn/staging
drwxrwx---   - hadoop supergroup          0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history
drwxrwx---   - hadoop supergroup          0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done
drwxrwxrwt   - hadoop supergroup          0 2016-11-21 13:41 /tmp/hadoop-yarn/staging/history/done_intermediate
drwxr-xr-x   - hadoop supergroup          0 2016-11-21 14:11 /user
drwxr-xr-x   - hadoop supergroup          0 2016-11-21 14:14 /user/hadoop
drwxr-xr-x   - hadoop supergroup          0 2016-11-21 14:14 /user/hadoop/input
```

*当然，你也可以使用下面的方式查看。*

```bash
[hadoop@master hadoop]$ hdfs dfs -ls 
Found 1 items
drwxr-xr-x   - hadoop supergroup          0 2016-11-21 14:14 input
```

*这种方式就类似在/user/hadoop目录下执行`ls`命令一样，这也从侧面反映了HDFS的默认工作目录就是我们在开始创建的/user/hadoop目录。*


#### 上传文件

这里还是使用单机模式是一的统计文件，文件内容如下：

```bash
[hadoop@master hadoop]$ cat input/sample.txt 
Do as I say, not as I do.
```

使用`hdfs dfs -put`命令将本地文件`input/sample.txt`上传到HDFS的`input`目录中。

```bash
[hadoop@master hadoop]$ hdfs dfs -put input/sample.txt input/ 
[hadoop@master hadoop]$ hdfs dfs -ls -R input
-rw-r--r--   1 hadoop supergroup         26 2016-11-21 14:16 input/sample.txt
```


### 运行Hadoop自带的统计单词的mapreduce程序`wordcount`

程序执行的具体情况如下：

```bash
[hadoop@master hadoop]$ hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.2.jar wordcount input output
16/11/21 14:17:23 INFO client.RMProxy: Connecting to ResourceManager at master/172.17.0.1:8032
16/11/21 14:17:25 INFO input.FileInputFormat: Total input paths to process : 1
16/11/21 14:17:25 INFO mapreduce.JobSubmitter: number of splits:1
16/11/21 14:17:25 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1479706884350_0001
16/11/21 14:17:26 INFO impl.YarnClientImpl: Submitted application application_1479706884350_0001
16/11/21 14:17:26 INFO mapreduce.Job: The url to track the job: http://master:8088/proxy/application_1479706884350_0001/
16/11/21 14:17:26 INFO mapreduce.Job: Running job: job_1479706884350_0001
16/11/21 14:17:37 INFO mapreduce.Job: Job job_1479706884350_0001 running in uber mode : false
16/11/21 14:17:37 INFO mapreduce.Job:  map 0% reduce 0%
16/11/21 14:17:45 INFO mapreduce.Job:  map 100% reduce 0%
16/11/21 14:17:52 INFO mapreduce.Job:  map 100% reduce 100%
16/11/21 14:17:52 INFO mapreduce.Job: Job job_1479706884350_0001 completed successfully
16/11/21 14:17:52 INFO mapreduce.Job: Counters: 49
        File System Counters
                FILE: Number of bytes read=63
                FILE: Number of bytes written=194213
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=138
                HDFS: Number of bytes written=33
                HDFS: Number of read operations=6
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=2
        Job Counters 
                Launched map tasks=1
                Launched reduce tasks=1
                Data-local map tasks=1
                Total time spent by all maps in occupied slots (ms)=4774
                Total time spent by all reduces in occupied slots (ms)=5318
                Total time spent by all map tasks (ms)=4774
                Total time spent by all reduce tasks (ms)=5318
                Total vcore-seconds taken by all map tasks=4774
                Total vcore-seconds taken by all reduce tasks=5318
                Total megabyte-seconds taken by all map tasks=4888576
                Total megabyte-seconds taken by all reduce tasks=5445632
        Map-Reduce Framework
                Map input records=1
                Map output records=8
                Map output bytes=58
                Map output materialized bytes=63
                Input split bytes=112
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
                GC time elapsed (ms)=145
                CPU time spent (ms)=1320
                Physical memory (bytes) snapshot=296816640
                Virtual memory (bytes) snapshot=1722683392
                Total committed heap usage (bytes)=136515584
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
[hadoop@master hadoop]$ hdfs dfs -cat output/*
Do      1
I       2
as      2
do.     1
not     1
say,    1
```

*可以看到这里执行mapreduce程序的方法和单机模式是完全一样的，区别仅仅在于读取文件的位置。单机模式的输入文件在Linux文件系统中，而全分布模式的输入文件中HDFS分布式文件系统中。*

*可以使用`hdfs dfs -get`命令将HDFS中的文件取回到本地来。如：将全分布模式下运行程序得到的结果`output`目录取回到本地并且重命名为hdfs_output。*

```bash
[hadoop@master hadoop]$ hdfs dfs -get output ./hdfs_output
[hadoop@master hadoop]$ cat hdfs_output/*
Do      1
I       2
as      2
do.     1
not     1
say,    1
```

*同单机模式一样，每次运行mapreduce时都需要确保HDFS当前工作目录下不能有output目录，否则程序会运行失败。删除HDFS的output目录的方法如下。*

```bash
[hadoop@master hadoop]$ hdfs dfs -rm -r output
16/11/21 14:20:13 INFO fs.TrashPolicyDefault: Namenode trash configuration: Deletion interval = 0 minutes, Emptier interval = 0 minutes.
Deleted output
```


### 网页工具

1. 查看集群信息：[http://master:50070/](http://master:50070/)
2. 查看集群任务：[http://master:8088/](http://master:8088/)

