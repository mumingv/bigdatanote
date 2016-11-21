# 初识Hadoop

## Hadoop的构造模块

Hadoop主要包含下面这些模块，本质上它们都是守护进程（daemon）。
1. NameNode：名字节点；
2. DataNode：数据节点；
3. Secondary NameNode：次名字节点；
4. JobTracker：作业跟踪节点；
5. TaskTracker：任务跟踪节点；

### NameNode

Hadoop在分布式计算和分布式存储中都采用了主从结构（master/slave）。分布式存储系统被称为Hadoop文件系统，简称为HDFS。

NameNode位于HDFS的主端，用来指导从端的DataNode执行底层的I/O任务。

NameNode相当于HDFS的大脑，它会跟踪文件如何被分割成文件块，且这些文件块存储在哪些节点上，以及会监控整个HDFS的运行状态是否正常。

由于每个集群只有一个NameNode，所以如果NameNode出现故障的话，整个Hadoop集群就无法使用了。为了解决这个问题，就需要引入一个节点来备份NameNode的信息，也就是后面要讲的Secondary NameNode。


### DataNode

Hadoop集群上的每一个从节点都会启动一个DataNode的守护进程，用来执行HDFS的繁重的工作。

客户端从NameNode处得知分割后的数据块存储在哪个DataNode上面，然后客户端就会直接和DataNode进行通信，来处理与数据块相对于的本地文件。同时，DataNode会与其他DataNode进行通信，复制这些数据块以做备份。


### Secondary NameNode

Secondary NameNode进程只会和NameNode进行通信，定期从NameNode获取HDFS的元数据快照。这样一来，当NameNode出现单点故障的话，就可以使用SNN（Secondary NameNode的缩写）保存的快照进行数据恢复。


### JobTracker

JobTracker是应用程序和Hadoop系统之间的接口。其用来给用户提交的任务分配节点并且监控任务的运行，任务失败的话还会自动重启任务。

每个Hadoop集群只有一个JobTracker进程，其通常和NameNode一起运行在主节点上。


### TaskTracker

TaskTracker用来管理任务在从节点上的执行情况，其会执行由JobTracker分配的单向任务。

每个从节点上只有一个TaskTracker，但是每个TaskTracker却可以生成多个JVM（Java虚拟机）来并行地处理许多map或reduce任务。

TaskTracker会定时发送心跳消息给JobTracker，如果JobTracker在一定时间内没有收到TaskTracker心跳的话，就会认为TaskTracker已经“阵亡”了，从而会将该TaskTracker执行的任务重新分配给其他节点的TaskTracker。


## 为Hadoop集群安装SSH

### 定义一个公共账号

参考：[创建Hadoop用户](#docs/install#创建Hadoop用户)。


### 设置集群各机器间免密登陆

参考：[设置机器间免密登陆](#docs/install#设置机器间免密登陆)。


## 运行Hadoop

### 本地（单机）模式

示例：[用Hadoop统计单词（单机模式）](#docs/hia_wordcount_standalone)。


### 伪分布模式

示例：[用Hadoop统计单词（伪分布模式）](#docs/hia_wordcount_pseudo)。


### 全分布（集群）模式

示例：[用Hadoop统计单词（全分布/集群模式）](#docs/hia_wordcount_cluster)。


## 基于Web的集群用户界面

参考：[]()。

## 小结

略。
