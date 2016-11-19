# Hadoop环境安装

**说明**：软件安装使用的系统是CentOS 7.0(Aliyun ECS)，其他Linux环境的安装方法请查阅相关文档，可能略有差异，本文不做说明。

## 安装JDK

### 基本概念

众所周知，整个Hadoop生态都是基于Java语言构建的，所以要安装Hadoop相关的软件，需要先安装Java语言包。

对于没有Java基础的开发者，需要先了解下面几个概念：
- JDK = Java Development Kit，Java软件开发包；
- JRE = Java Runtime Environment，Java运行环境；
- J2ME = Java 2 Micro Edition，Java2微型版；
- J2SE = Java 2 Standard Edition，Java2标准版；
- J2EE = Java 2 Enterprise Edition，Java2企业版；

JDK用于开发Java程序，而JRE用于运行Java程序。**如果安装了JDK的话，就不用再安装JRE了，因为JDK中是包含JRE软件包的。**

对于J2ME、J2SE、J2EE这三者，对于普通的应用程序开发而言，使用J2SE即可。**本文Hadoop使用的Java开发环境就是J2SE的JDK。**

另外，相关的概念还有：
- javac = Java Compiler，Java编译器；
- jvm = Java Virtual Machine，Java虚拟机；

Java编译器(javac)用于编译生成中间文件，而Java虚拟机(jvm)用于将中间文件解释成机器语言，最后编译成功的可执行文件在Java虚拟机(jvm)中运行。

### 安装方法

对于jdk的安装，通常有下面三种方法，<font color=#FF0000>本文采用的是“方法三”</font>。这里会详细介绍第三种安装方法，另外两种方法只做简要介绍。

### 方法一：使用CentOS自带的源进行安装(yum方式)

**优点**：CentOS 7系统自带各版本的openjdk，安装比较简便；

**缺点**：该版本是单独维护的一个jdk分支，并非oracle的官方版本，功能上有所缺失。比如：安装后的lib目录下没有dt.jar和tools.jar这两个jar包。

查询可用的java版本，可以看到对应的jdk软件包名称为`java-1.7.0-openjdk`。

```bash
$ yum list | grep ^java-1.7.0
java-1.7.0-openjdk.x86_64               1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-accessibility.x86_64 1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-demo.x86_64          1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-devel.x86_64         1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-headless.x86_64      1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-javadoc.noarch       1:1.7.0.111-2.6.7.2.el7_2      updates  
java-1.7.0-openjdk-src.x86_64           1:1.7.0.111-2.6.7.2.el7_2      updates 
```

具体安装的话，使用如下命令即可：

```bash
$ sudo yum install java-1.7.0-openjdk
```

安装成功后，需要设置`JAVA_HOME`等环境变量，具体操作


### 方法二：下载JDK压缩包解压安装

**优点**：直接下载解压，不需要编译。

**缺点**：
1. 需要手工下载软件包；
2. 需要将jdk目录设置到环境变量`PATH`中，否则无法直接使用`java`等命令。

64位二进制软件安装包：`jdk-7u80-linux-x64.tar.gz`。

注：如果是32位机器的话，对应名称为`jdk-7u80-linux-i586.tar.gz`。


### 方法三：下载rpm包进行安装（<font color="red">本文使用</font>）

**优点**：直接下载，然后使用`rpm -ivh`命令进行安装，无需编译。

**缺点**：需要手工下载软件包。

具体安装步骤如下：

#### 下载rpm软件包

使用的jdk版本为1.7.80，软件包可以使用`wget`命令进行下载

```bash
$ wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.rpm
```

或者使用`curl`命令进行下载

```bash
$ curl -v -j -k -L -H "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jdk-7u80-linux-x64.rpm > jdk-7u80-linux-x64.rpm
```
`curl`命令参数的含义：
```
-j -> junk cookies
-k -> ignore certificates
-L -> follow redirects
-H [arg] -> headers
```

#### 安装jdk

使用`rpm -ivh`命令进行安装

```bash
$ rpm -ivh jdk-7u80-linux-x64.rpm
```

安装完成后，通过如下命令查询

```
$ yum list installed | grep jdk
jdk.x86_64                         2000:1.7.0_80-fcs                   installed
```

#### 设置环境变量

在系统配置文件`/etc/profile`的末尾增加如下环境变量

```bash
# JAVA环境变量
JAVA_HOME=/usr/java/jdk1.7.0_80
JRE_HOME=/usr/java/jdk1.7.0_80/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib 
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin 
export JAVA_HOME JRE_HOME CLASSPATH PATH
```

激活环境变量

```bash
$ source /etc/profile
```

#### 确认jdk是否安装成功

如果已正确安装jdk的话，就可以查看到java的版本，如下：

```bash
$ java -version
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
```


### 参考资料

- [Apache软件镜像站点](http://mirrors.cnnic.cn/)
- [清华大学软件镜像站点](https://mirrors.tuna.tsinghua.edu.cn/)（该站点包含了apache软件镜像站点的内容）
- [各版本的jdk/jre镜像站点](https://mirror.its.sfu.ca/mirror/CentOS-Third-Party/NSG/common/x86_64/)
- [官方安装指导](http://docs.oracle.com/javase/8/docs/technotes/guides/install/linux_jdk.html#BJFJJEFG)
- [CentOS下安装JDK的三种方法](http://os.51cto.com/art/201609/517037.htm)


## 安装Hadoop

### 单机模式安装步骤

#### 创建Hadoop用户

1.使用root账号，创建一个名为hadoop的用户

```
# useradd -m hadoop -G root -s /bin/bash
```

说明；参数`-G root`表示hadoop作为`root`用户组的一员，会在`/etc/group`配置文件中体现。

```bash
$ cat /etc/group
root:x:0:hadoop  # '-G root'参数使用后，会多出来这一行。
hadoop:x:1006:  # 由于没有使用'-g'参数指定hadoop用户所属的组，所以hadoop的组名默认和用户名相同。参考'man useradd'手册关于'-g'参数的说明。
```

2.给新创建的hadoop用户设置密码

```
# passwd
Changing password for user root.
New password:  # 这里输入密码
Retype new password:  # 再输一次密码
```

这样就可以使用hadoop用户登陆了。

3.[可选]给hadoop用户增加管理员权限

执行`visudo`命令

```
# visudo
```

在打开的配置文件中，增加一行hadoop用户的配置

```
## Allow root to run any commands anywhere
root    ALL=(ALL)   ALL
%hadoop   ALL=(ALL)   ALL  # 增加这一行
```

这样就可以使用hadoop用户安装各种软件了，不过需要在执行的命令之前加上关键字`sudo`，表示以管理员root的身份执行该命令。如：

```bash
$ sudo yum install gcc
```


#### 设置机器间免密登陆

本节示例基于hadoop用户进行操作。

1.安装ssh

查看openssh软件是否安装，命令如下：

```bash
$ yum list installed | grep ssh
libssh2.x86_64                     1.4.3-8.el7                         @anaconda
openssh.x86_64                     6.4p1-8.el7                         @anaconda
openssh-clients.x86_64             6.4p1-8.el7                         @anaconda
openssh-server.x86_64              6.4p1-8.el7                         @anaconda
```

如果没有`openssh-clients`和`openssh-server`，则需要安装ssh。安装命令如下：

```bash
$ sudo yun install openssh
[sudo] password for hadoop:  # 这里输入hadoop用户的密码
```
```bash
$ sudo yum install openssh-clients
$ sudo yum install openssh-server
```

2.生成密钥

使用`ssh-keygen`命令生成密钥时会有一些用户提示，一般不用理会，一路回车即可。

```bash
$ ssh-keygen -t rsa
```

命令会生成两个密钥，一个私钥(id_rsa)，一个公钥(id_rsa.pub)。私钥和公钥是配对的，区别在于，私钥不能给别人，而公钥是可以给别人的。

```bash
$ ls ~/.ssh/
id_rsa  id_rsa.pub
```

3.设置免密登陆

假设机器A需要免密登陆机器B，则需要将机器A的公钥发送给机器B，即在机器B上需要保存机器A的公钥信息。

具体做法是：在机器B上执行如下命令

```bash
$ cat id_rsa.pub.of.A >> ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/authorized_keys
```

其含义是：机器B授权机器A能够免密登陆。

<font color="red">
重要说明：<br />
a. 一定要将authorized_keys文件的权限改为600，否则该文件不会生效，也就无法进行免密登陆。<br />
b. 机器B需要免密登陆机器A的话，同样在机器A上需要保存机器B的公钥信息。<br />
c. Hadoop集群中机器两两之间是需要使用ssh进行通信的，所有每台机器都需要加上其他机器的公钥信息。<br />
</font>

4.使用ssh进行登陆

设置完成后，就可以直接使用ssh免密登陆了。如，从机器A上使用`ssh`命令登陆机器B：

```bash
$ ssh 192.168.1.102  # 机器B的IP
```


#### 安装Hadoop2

本节示例基于hadoop用户进行操作。

1.使用hadoop-2.5.2版本：[下载](http://mirrors.cnnic.cn/apache/hadoop/common/hadoop-2.5.2/hadoop-2.5.2.tar.gz)。

安装包下载后放在`/usr/local/src`目录下。如下：

```bash
$ ls -l /usr/local/src/hadoop*
-rw-rw-r-- 1 work work 147197492 Nov 16 21:14 /usr/local/src/hadoop-2.5.2.tar.gz
```

2.安装hadoop

```bash
$ sudo tar -xzf /usr/local/src/hadoop-2.5.2.tar.gz -C /usr/local
$ sudo mv /usr/local/src/hadoop-2.5.2 /usr/local/src/hadoop
$ sudo chown -R hadoop:hadoop /usr/local/src/hadoop
```

3.检查hadoop是否可用

使用如下命令查看hadoop版本，如果显示版本信息则表明安装成功。

```bash
$ /usr/local/hadoop/bin/hadoop version
Hadoop 2.5.2
Subversion https://git-wip-us.apache.org/repos/asf/hadoop.git -r cc72e9b000545b86b75a61f4835eb86d57bfafc0
Compiled by jenkins on 2014-11-14T23:45Z
Compiled with protoc 2.5.0
From source with checksum df7537a4faa4658983d397abf4514320
This command was run using /usr/local/hadoop/share/hadoop/common/hadoop-common-2.5.2.jar
```

4.设置环境变量

在系统配置文件`/etc/profile`的末尾增加如下环境变量

```bash
# Hadoop环境变量
HADOOP_HOME=/usr/local/hadoop
CLASSPATH=$($HADOOP_HOME/bin/hadoop classpath):$CLASSPATH
PATH=$PATH:$HADOOP_HOME/bin
export HADOOP_HOME CLASSPATH PATH
```

设置好环境变量后，执行如下命令使配置生效。
```bash
[hadoop@CentOS ~]$ source /etc/profile
```

5.一个小例子

Hadoop单机模式安装成功后，就可以试试下面这个示例体验一下了。

示例：[用Hadoop统计单词](#docs/hia_01#运行第一个程序（用Hadoop统计单词）)。


### 伪分布模式安装步骤

*<font color="red">说明：在此之前，请先确保已经完成了[单机模式的安装](#docs/install#单机模式安装步骤)。</font>另外，本节所涉及的所有命令均使用hadoop用户执行。*

#### 设置环境变量

在系统配置文件`/etc/profile`的末尾增加如下环境变量

```bash
# Hadoop环境变量（伪分布式）
HADOOP_INSTALL=$HADOOP_HOME
HADOOP_MAPRED_HOME=$HADOOP_HOME
HADOOP_COMMON_HOME=$HADOOP_HOME
HADOOP_HDFS_HOME=$HADOOP_HOME
YARN_HOME=$HADOOP_HOME
HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
PATH=$PATH:$HADOOP_HOME/sbin
export HADOOP_INSTALL HADOOP_MAPRED_HOME HADOOP_COMMON_HOME
export HADOOP_HDFS_HOME YARN_HOME HADOOP_COMMON_LIB_NATIVE_DIR PATH
```

设置好环境变量后，执行如下命令使配置生效。
```bash
[hadoop@CentOS ~]$ source /etc/profile
```


#### 修改Hadoop配置

*说明：本节所涉及的配置均在`/usr/local/hadoop/etc/hadoop`目录下。*

1.修改Hadoop中的环境变量配置

将文件`hadoop-env.sh`中的
```
export JAVA_HOME=${JAVA_HOME}
```
修改为
```
export JAVA_HOME=/usr/java/jdk1.7.0_80
```

*说明：如果不修改JAVA_HOME的话，在启动NameNode和DataNode的时候，会报如下错误导致启动不成功。*

```bash
[hadoop@CentOS hadoop]$ ./sbin/start-dfs.sh
16/11/19 16:47:58 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Starting namenodes on [localhost]
localhost: Error: JAVA_HOME is not set and could not be found.
localhost: Error: JAVA_HOME is not set and could not be found.
Starting secondary namenodes [0.0.0.0]
0.0.0.0: Error: JAVA_HOME is not set and could not be found.
16/11/19 16:48:04 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```

2.修改配置文件

将文件`core-site.xml`中的
```
<configuration>
</configuration>
```
修改为
```
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/usr/local/hadoop/tmp</value>
        <description>A base for other temporary directories.</description>
    </property>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```

将文件`hdfs-site.xml`中的
```
<configuration>
</configuration>
```
修改为
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/usr/local/hadoop/tmp/dfs/data</value>
    </property>
</configuration>
```

3.格式化NameNode

执行命令

```bash
[hadoop@CentOS ~]$ cd /usr/local/hadoop/
[hadoop@CentOS hadoop]$ ./bin/hdfs namenode -format
```

输出结果中出现`common.Storage: Storage directory /usr/local/hadoop/tmp/dfs/name has been successfully`和`util.ExitUtil: Exiting with status 0`，则表明NameNode格式化成功。命令的具体输出如下：

```
16/11/19 17:25:01 INFO namenode.NameNode: STARTUP_MSG: 
/************************************************************
STARTUP_MSG: Starting NameNode
STARTUP_MSG:   host = <hostname>/172.17.0.1
STARTUP_MSG:   args = [-format]
STARTUP_MSG:   version = 2.5.2
STARTUP_MSG:   classpath = /usr/local/hadoop/etc/hadoop:/usr/local/hadoop/share/hadoop/common/lib/commons-logging-1.1.3.jar:...
STARTUP_MSG:   build = https://git-wip-us.apache.org/repos/asf/hadoop.git -r cc72e9b000545b86b75a61f4835eb86d57bfafc0; compiled by 'jenkins' on 2014-11-14T23:45Z
STARTUP_MSG:   java = 1.7.0_80
************************************************************/
16/11/19 17:25:01 INFO namenode.NameNode: registered UNIX signal handlers for [TERM, HUP, INT]
16/11/19 17:25:01 INFO namenode.NameNode: createNameNode [-format]
16/11/19 17:25:01 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Formatting using clusterid: CID-c0c25db2-a5aa-48b6-8d28-1cd6cccc4434
16/11/19 17:25:02 INFO namenode.FSNamesystem: fsLock is fair:true
16/11/19 17:25:02 INFO blockmanagement.DatanodeManager: dfs.block.invalidate.limit=1000
16/11/19 17:25:02 INFO blockmanagement.DatanodeManager: dfs.namenode.datanode.registration.ip-hostname-check=true
16/11/19 17:25:02 INFO blockmanagement.BlockManager: dfs.namenode.startup.delay.block.deletion.sec is set to 000:00:00:00.000
16/11/19 17:25:02 INFO blockmanagement.BlockManager: The block deletion will start around 2016 Nov 19 17:25:02
16/11/19 17:25:02 INFO util.GSet: Computing capacity for map BlocksMap
16/11/19 17:25:02 INFO util.GSet: VM type       = 64-bit
16/11/19 17:25:02 INFO util.GSet: 2.0% max memory 966.7 MB = 19.3 MB
16/11/19 17:25:02 INFO util.GSet: capacity      = 2^21 = 2097152 entries
16/11/19 17:25:02 INFO blockmanagement.BlockManager: dfs.block.access.token.enable=false
16/11/19 17:25:02 INFO blockmanagement.BlockManager: defaultReplication         = 1
16/11/19 17:25:02 INFO blockmanagement.BlockManager: maxReplication             = 512
16/11/19 17:25:02 INFO blockmanagement.BlockManager: minReplication             = 1
16/11/19 17:25:02 INFO blockmanagement.BlockManager: maxReplicationStreams      = 2
16/11/19 17:25:02 INFO blockmanagement.BlockManager: shouldCheckForEnoughRacks  = false
16/11/19 17:25:02 INFO blockmanagement.BlockManager: replicationRecheckInterval = 3000
16/11/19 17:25:02 INFO blockmanagement.BlockManager: encryptDataTransfer        = false
16/11/19 17:25:02 INFO blockmanagement.BlockManager: maxNumBlocksToLog          = 1000
16/11/19 17:25:02 INFO namenode.FSNamesystem: fsOwner             = hadoop (auth:SIMPLE)
16/11/19 17:25:02 INFO namenode.FSNamesystem: supergroup          = supergroup
16/11/19 17:25:02 INFO namenode.FSNamesystem: isPermissionEnabled = true
16/11/19 17:25:02 INFO namenode.FSNamesystem: HA Enabled: false
16/11/19 17:25:02 INFO namenode.FSNamesystem: Append Enabled: true
16/11/19 17:25:03 INFO util.GSet: Computing capacity for map INodeMap
16/11/19 17:25:03 INFO util.GSet: VM type       = 64-bit
16/11/19 17:25:03 INFO util.GSet: 1.0% max memory 966.7 MB = 9.7 MB
16/11/19 17:25:03 INFO util.GSet: capacity      = 2^20 = 1048576 entries
16/11/19 17:25:03 INFO namenode.NameNode: Caching file names occuring more than 10 times
16/11/19 17:25:03 INFO util.GSet: Computing capacity for map cachedBlocks
16/11/19 17:25:03 INFO util.GSet: VM type       = 64-bit
16/11/19 17:25:03 INFO util.GSet: 0.25% max memory 966.7 MB = 2.4 MB
16/11/19 17:25:03 INFO util.GSet: capacity      = 2^18 = 262144 entries
16/11/19 17:25:03 INFO namenode.FSNamesystem: dfs.namenode.safemode.threshold-pct = 0.9990000128746033
16/11/19 17:25:03 INFO namenode.FSNamesystem: dfs.namenode.safemode.min.datanodes = 0
16/11/19 17:25:03 INFO namenode.FSNamesystem: dfs.namenode.safemode.extension     = 30000
16/11/19 17:25:03 INFO namenode.FSNamesystem: Retry cache on namenode is enabled
16/11/19 17:25:03 INFO namenode.FSNamesystem: Retry cache will use 0.03 of total heap and retry cache entry expiry time is 600000 millis
16/11/19 17:25:03 INFO util.GSet: Computing capacity for map NameNodeRetryCache
16/11/19 17:25:03 INFO util.GSet: VM type       = 64-bit
16/11/19 17:25:03 INFO util.GSet: 0.029999999329447746% max memory 966.7 MB = 297.0 KB
16/11/19 17:25:03 INFO util.GSet: capacity      = 2^15 = 32768 entries
16/11/19 17:25:03 INFO namenode.NNConf: ACLs enabled? false
16/11/19 17:25:03 INFO namenode.NNConf: XAttrs enabled? true
16/11/19 17:25:03 INFO namenode.NNConf: Maximum size of an xattr: 16384
16/11/19 17:25:03 INFO namenode.FSImage: Allocated new BlockPoolId: BP-1431225708-172.17.0.1-1479547503226
16/11/19 17:25:03 INFO common.Storage: Storage directory /usr/local/hadoop/tmp/dfs/name has been successfully formatted.
16/11/19 17:25:03 INFO namenode.NNStorageRetentionManager: Going to retain 1 images with txid >= 0
16/11/19 17:25:03 INFO util.ExitUtil: Exiting with status 0
16/11/19 17:25:03 INFO namenode.NameNode: SHUTDOWN_MSG: 
/************************************************************
SHUTDOWN_MSG: Shutting down NameNode at <hostname>/172.17.0.1
************************************************************/
```

格式化成功后会生成存放`NameNode`、`SecondaryNameNode`以及`DataNode`的目录
```
[hadoop@CentOS hadoop]$ ls tmp/dfs/
data  name  namesecondary
```
以及存放日志的目录
```
[hadoop@CentOS hadoop]$ ls -1 logs/
hadoop-hadoop-datanode-<hostname>.log
hadoop-hadoop-datanode-<hostname>.out
hadoop-hadoop-namenode-<hostname>.log
hadoop-hadoop-namenode-<hostname>.out
hadoop-hadoop-secondarynamenode-<hostname>.log
hadoop-hadoop-secondarynamenode-<hostname>.out
SecurityAuth-hadoop.audit
```
*说明：如果启动失败或者运行过程中出现问题，可以优先到logs目录下查看日志以确认问题的可能原因。*

4.启动`NameNode`、`SecondaryNameNode`以及`DataNode`的守护进程

执行`start-dfs.sh`脚本运行守护进程：

```
[hadoop@CentOS hadoop]$ ./sbin/start-dfs.sh          
16/11/19 17:32:40 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
Starting namenodes on [localhost]
localhost: starting namenode, logging to /usr/local/hadoop/logs/hadoop-hadoop-namenode-iZ25sle0t20Z.out
localhost: starting datanode, logging to /usr/local/hadoop/logs/hadoop-hadoop-datanode-iZ25sle0t20Z.out
Starting secondary namenodes [0.0.0.0]
0.0.0.0: starting secondarynamenode, logging to /usr/local/hadoop/logs/hadoop-hadoop-secondarynamenode-iZ25sle0t20Z.out
16/11/19 17:33:00 WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```

执行`jps`命令查询进程是否启动成功，成功的话会显示各Node的名称及其对应的进程号：

```
[hadoop@CentOS hadoop]$ jps
13321 NameNode
13600 SecondaryNameNode
13419 DataNode
13705 Jps
```

*说明：`jps`命令是Java提供的工具，该命令只能查看Java的进程。当然你也可以使用Linux系统自带的命令`ps`进行查询，如下所示：*

```
[hadoop@CentOS hadoop]$ ps uxf
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...
hadoop   13600  2.3  6.0 1546556 113256 ?      Sl   17:53   0:05 /usr/java/jdk1.7.0_80/bin/java -Dproc_secondarynamenode -Xmx1000m -D
hadoop   13419  2.0  4.9 1568084 92720 ?       Sl   17:53   0:04 /usr/java/jdk1.7.0_80/bin/java -Dproc_datanode -Xmx1000m -Djava.net.
hadoop   13321  2.4  6.9 1568060 131192 ?      Sl   17:53   0:05 /usr/java/jdk1.7.0_80/bin/java -Dproc_namenode -Xmx1000m -Djava.net.
```

*说明：我们也可以通过`netstat`命令查看各个Hadoop的Java进程使用了哪些端口。*

```
[hadoop@CentOS hadoop]$ netstat -tunlp | grep java
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp  0  0 127.0.0.1:9000  0.0.0.0:*  LISTEN  13321/java          
tcp  0  0 0.0.0.0:50070   0.0.0.0:*  LISTEN  13321/java 
tcp  0  0 0.0.0.0:50090   0.0.0.0:*  LISTEN  13600/java          
tcp  0  0 0.0.0.0:50010   0.0.0.0:*  LISTEN  13419/java          
tcp  0  0 0.0.0.0:50020   0.0.0.0:*  LISTEN  13419/java          
tcp  0  0 0.0.0.0:50075   0.0.0.0:*  LISTEN  13419/java          
```
*可以看到NameNode（13321）进程开了2个TCP端口（9000和50070）。SecondaryNameNode（13600）进程开了1个TCP端口（50090）。DataNode（13419）进程开了3个TCP端口（50010、50020和50075）。其中，9000端口是我们自己在core-site.xml配置文件中指定的，其他端口都是Hadoop默认指定的。*

*如果你比较细心的话，会发现NameNode开的两个端口有些不太一样，一个是`127.0.0.1:9000`，另一个是却是`0.0.0.0:50070`。其中50070端口是可以通过外网进行访问的，而9000端口却不能。*

*说明：你一定会想知道如何停止NameNode、SecondaryNameNode以及DataNode的守护进程，很简单，执行下面的脚本即可。*

```bash
[hadoop@CentOS hadoop]$ ./sbin/stop-dfs.sh
```

5.页面查看Hadoop系统状态

在浏览器中输入`http://<IP>:50070/`即可，这里的`<IP>`是你部署Hadoop的Linux主机的IP地址。如下图所示：

[![查看Hadoop系统状态](images/bigdatanote_hadoop_install.png)](images/bigdatanote_hadoop_install.png)

*说明：在该页面中可以查看NameNode、DataNode的信息，以及HDFS文件和日志文件。具体细节请自行研究。*

6.一个小例子

Hadoop伪分布式模式安装成功后，就可以试试下面这个示例体验一下了。

示例：[例子]()。


### 参考资料

- [安装Hadoop](http://www.powerxing.com/install-hadoop-in-centos/)("安装Java环境"一节不用看，不推荐使用openjdk）
- [Hadoop官方网站](http://hadoop.apache.org/)
- [厦门大学数据库实验室](http://dblab.xmu.edu.cn/)(厦门大学有关大数据的研究资料和教学视频)
- [设置机器间免密登陆](http://blog.csdn.net/dongwuming/article/details/9705595)


