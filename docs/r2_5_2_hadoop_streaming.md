# hadoop streaming

## Hadoop Streaming 介绍

Hadoop支持使用可执行文件或者脚本作为mapper和reducer。示例如下： 

假设HDFS的input目录中有一个sample.txt文件，现在要统计该文件的行数。该文件的内容如下。

```bash
$ hadoop fs -ls -R     
drwxr-xr-x   - hadoop supergroup          0 2016-12-17 17:50 input
-rw-r--r--   1 hadoop supergroup         26 2016-12-17 17:50 input/sample.txt
$ hadoop fs -cat input/sample.txt
Do as I say, not as I do.
```

使用`cat`命令作为mapper，`wc`命令作为reducer。运行hadoop streaming，如下所示：

```bash
$ hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-2.5.2.jar -input input -output output -mapper /usr/bin/cat -reducer /usr/bin/wc  
16/12/17 17:52:38 INFO Configuration.deprecation: session.id is deprecated. Instead, use dfs.metrics.session-id
16/12/17 17:52:38 INFO jvm.JvmMetrics: Initializing JVM Metrics with processName=JobTracker, sessionId=
16/12/17 17:52:38 INFO jvm.JvmMetrics: Cannot initialize JVM Metrics with processName=JobTracker, sessionId= - already initialized
16/12/17 17:52:39 INFO mapred.FileInputFormat: Total input paths to process : 1
16/12/17 17:52:40 INFO mapreduce.JobSubmitter: number of splits:1
16/12/17 17:52:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_local900968006_0001
16/12/17 17:52:40 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/staging/hadoop900968006/.staging/job_local900968006_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/12/17 17:52:40 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/staging/hadoop900968006/.staging/job_local900968006_0001/job.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/12/17 17:52:41 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/local/localRunner/hadoop/job_local900968006_0001/job_local900968006_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.retry.interval;  Ignoring.
16/12/17 17:52:41 WARN conf.Configuration: file:/usr/local/hadoop/tmp/mapred/local/localRunner/hadoop/job_local900968006_0001/job_local900968006_0001.xml:an attempt to override final parameter: mapreduce.job.end-notification.max.attempts;  Ignoring.
16/12/17 17:52:41 INFO mapreduce.Job: The url to track the job: http://localhost:8080/
16/12/17 17:52:41 INFO mapred.LocalJobRunner: OutputCommitter set in config null
16/12/17 17:52:41 INFO mapreduce.Job: Running job: job_local900968006_0001
16/12/17 17:52:41 INFO mapred.LocalJobRunner: OutputCommitter is org.apache.hadoop.mapred.FileOutputCommitter
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Waiting for map tasks
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Starting task: attempt_local900968006_0001_m_000000_0
16/12/17 17:52:41 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/12/17 17:52:41 INFO mapred.MapTask: Processing split: hdfs://localhost:9000/user/hadoop/input/sample.txt:0+26
16/12/17 17:52:41 INFO mapred.MapTask: numReduceTasks: 1
16/12/17 17:52:41 INFO mapred.MapTask: Map output collector class = org.apache.hadoop.mapred.MapTask$MapOutputBuffer
16/12/17 17:52:41 INFO mapred.MapTask: (EQUATOR) 0 kvi 26214396(104857584)
16/12/17 17:52:41 INFO mapred.MapTask: mapreduce.task.io.sort.mb: 100
16/12/17 17:52:41 INFO mapred.MapTask: soft limit at 83886080
16/12/17 17:52:41 INFO mapred.MapTask: bufstart = 0; bufvoid = 104857600
16/12/17 17:52:41 INFO mapred.MapTask: kvstart = 26214396; length = 6553600
16/12/17 17:52:41 INFO streaming.PipeMapRed: PipeMapRed exec [/usr/bin/cat]
16/12/17 17:52:41 INFO streaming.PipeMapRed: R/W/S=1/0/0 in:NA [rec/s] out:NA [rec/s]
16/12/17 17:52:41 INFO streaming.PipeMapRed: Records R/W=1/1
16/12/17 17:52:41 INFO streaming.PipeMapRed: MRErrorThread done
16/12/17 17:52:41 INFO streaming.PipeMapRed: mapRedFinished
16/12/17 17:52:41 INFO mapred.LocalJobRunner: 
16/12/17 17:52:41 INFO mapred.MapTask: Starting flush of map output
16/12/17 17:52:41 INFO mapred.MapTask: Spilling map output
16/12/17 17:52:41 INFO mapred.MapTask: bufstart = 0; bufend = 27; bufvoid = 104857600
16/12/17 17:52:41 INFO mapred.MapTask: kvstart = 26214396(104857584); kvend = 26214396(104857584); length = 1/6553600
16/12/17 17:52:41 INFO mapred.MapTask: Finished spill 0
16/12/17 17:52:41 INFO mapred.Task: Task:attempt_local900968006_0001_m_000000_0 is done. And is in the process of committing
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Records R/W=1/1
16/12/17 17:52:41 INFO mapred.Task: Task 'attempt_local900968006_0001_m_000000_0' done.
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Finishing task: attempt_local900968006_0001_m_000000_0
16/12/17 17:52:41 INFO mapred.LocalJobRunner: map task executor complete.
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Waiting for reduce tasks
16/12/17 17:52:41 INFO mapred.LocalJobRunner: Starting task: attempt_local900968006_0001_r_000000_0
16/12/17 17:52:41 INFO mapred.Task:  Using ResourceCalculatorProcessTree : [ ]
16/12/17 17:52:41 INFO mapred.ReduceTask: Using ShuffleConsumerPlugin: org.apache.hadoop.mapreduce.task.reduce.Shuffle@9f012b8
16/12/17 17:52:41 INFO reduce.MergeManagerImpl: MergerManager: memoryLimit=363285696, maxSingleShuffleLimit=90821424, mergeThreshold=239768576, ioSortFactor=10, memToMemMergeOutputsThreshold=10
16/12/17 17:52:41 INFO reduce.EventFetcher: attempt_local900968006_0001_r_000000_0 Thread started: EventFetcher for fetching Map Completion Events
16/12/17 17:52:41 INFO reduce.LocalFetcher: localfetcher#1 about to shuffle output of map attempt_local900968006_0001_m_000000_0 decomp: 31 len: 35 to MEMORY
16/12/17 17:52:41 INFO reduce.InMemoryMapOutput: Read 31 bytes from map-output for attempt_local900968006_0001_m_000000_0
16/12/17 17:52:41 INFO reduce.MergeManagerImpl: closeInMemoryFile -> map-output of size: 31, inMemoryMapOutputs.size() -> 1, commitMemory -> 0, usedMemory ->31
16/12/17 17:52:41 WARN io.ReadaheadPool: Failed readahead on ifile
EBADF: Bad file descriptor
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX.posix_fadvise(Native Method)
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX.posixFadviseIfPossible(NativeIO.java:263)
        at org.apache.hadoop.io.nativeio.NativeIO$POSIX$CacheManipulator.posixFadviseIfPossible(NativeIO.java:142)
        at org.apache.hadoop.io.ReadaheadPool$ReadaheadRequestImpl.run(ReadaheadPool.java:206)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
        at java.lang.Thread.run(Thread.java:745)
16/12/17 17:52:41 INFO reduce.EventFetcher: EventFetcher is interrupted.. Returning
16/12/17 17:52:41 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/12/17 17:52:41 INFO reduce.MergeManagerImpl: finalMerge called with 1 in-memory map-outputs and 0 on-disk map-outputs
16/12/17 17:52:42 INFO mapred.Merger: Merging 1 sorted segments
16/12/17 17:52:42 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 3 bytes
16/12/17 17:52:42 INFO reduce.MergeManagerImpl: Merged 1 segments, 31 bytes to disk to satisfy reduce memory limit
16/12/17 17:52:42 INFO reduce.MergeManagerImpl: Merging 1 files, 35 bytes from disk
16/12/17 17:52:42 INFO reduce.MergeManagerImpl: Merging 0 segments, 0 bytes from memory into reduce
16/12/17 17:52:42 INFO mapred.Merger: Merging 1 sorted segments
16/12/17 17:52:42 INFO mapred.Merger: Down to the last merge-pass, with 1 segments left of total size: 3 bytes
16/12/17 17:52:42 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/12/17 17:52:42 INFO streaming.PipeMapRed: PipeMapRed exec [/usr/bin/wc]
16/12/17 17:52:42 INFO streaming.PipeMapRed: R/W/S=1/0/0 in:NA [rec/s] out:NA [rec/s]
16/12/17 17:52:42 INFO streaming.PipeMapRed: Records R/W=1/1
16/12/17 17:52:42 INFO streaming.PipeMapRed: MRErrorThread done
16/12/17 17:52:42 INFO streaming.PipeMapRed: mapRedFinished
16/12/17 17:52:42 INFO mapreduce.Job: Job job_local900968006_0001 running in uber mode : false
16/12/17 17:52:42 INFO mapreduce.Job:  map 100% reduce 0%
16/12/17 17:52:42 INFO mapred.Task: Task:attempt_local900968006_0001_r_000000_0 is done. And is in the process of committing
16/12/17 17:52:42 INFO mapred.LocalJobRunner: 1 / 1 copied.
16/12/17 17:52:42 INFO mapred.Task: Task attempt_local900968006_0001_r_000000_0 is allowed to commit now
16/12/17 17:52:42 INFO output.FileOutputCommitter: Saved output of task 'attempt_local900968006_0001_r_000000_0' to hdfs://localhost:9000/user/hadoop/output/_temporary/0/task_local900968006_0001_r_000000
16/12/17 17:52:42 INFO mapred.LocalJobRunner: Records R/W=1/1 > reduce
16/12/17 17:52:42 INFO mapred.Task: Task 'attempt_local900968006_0001_r_000000_0' done.
16/12/17 17:52:42 INFO mapred.LocalJobRunner: Finishing task: attempt_local900968006_0001_r_000000_0
16/12/17 17:52:42 INFO mapred.LocalJobRunner: reduce task executor complete.
16/12/17 17:52:43 INFO mapreduce.Job:  map 100% reduce 100%
16/12/17 17:52:43 INFO mapreduce.Job: Job job_local900968006_0001 completed successfully
16/12/17 17:52:43 INFO mapreduce.Job: Counters: 38
        File System Counters
                FILE: Number of bytes read=210226
                FILE: Number of bytes written=687013
                FILE: Number of read operations=0
                FILE: Number of large read operations=0
                FILE: Number of write operations=0
                HDFS: Number of bytes read=52
                HDFS: Number of bytes written=25
                HDFS: Number of read operations=15
                HDFS: Number of large read operations=0
                HDFS: Number of write operations=4
        Map-Reduce Framework
                Map input records=1
                Map output records=1
                Map output bytes=27
                Map output materialized bytes=35
                Input split bytes=102
                Combine input records=0
                Combine output records=0
                Reduce input groups=1
                Reduce shuffle bytes=35
                Reduce input records=1
                Reduce output records=1
                Spilled Records=2
                Shuffled Maps =1
                Failed Shuffles=0
                Merged Map outputs=1
                GC time elapsed (ms)=61
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
                Bytes Written=25
16/12/17 17:52:43 INFO streaming.StreamJob: Output directory: output
```

最后一行说明已经将结果保存到了output目录，查看output目录得知生成的结果文件为`output/part-00000`。

```bash
$ hadoop fs -ls -R
drwxr-xr-x   - hadoop supergroup          0 2016-12-17 18:10 input
-rw-r--r--   1 hadoop supergroup         26 2016-12-17 18:09 input/sample.txt
drwxr-xr-x   - hadoop supergroup          0 2016-12-17 18:10 output
-rw-r--r--   1 hadoop supergroup          0 2016-12-17 18:10 output/_SUCCESS
-rw-r--r--   1 hadoop supergroup         25 2016-12-17 18:10 output/part-00000
```

```bash
$ hadoop fs -cat output/part-00000
      1       8      27
```

结果中的第一列的值`1`表示统计的文件行数，第二列的值`2`表示统计的文件单词数。第三列是什么？


## How Streaming Works 如何工作

略。


## Streaming Command Options 命令选项

hadoop命令格式如下。

```
$ hadoop command [genericOptions] [streamingOptions]
```

<font color="red">注意：streaming选项一定要放在通用选项后面，否则会有问题。</font>

streaming选项中有4个必选选项，其他均为可选选项。这4个必选选项为；
0. -input directoryname or filename
0. -output directoryname
0. -mapper executable or JavaClassName
0. -reducer executable or JavaClassName


## Generic Command Options 通用命令选项

略。


## More Usage Examples 示例

略。


## Frequently Asked Questions 常见问题


