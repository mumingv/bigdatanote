# 环境安装

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

```
# JAVA环境变量
JAVA_HOME=/usr/java/jdk1.7.0_80
JRE_HOME=/usr/java/jdk1.7.0_80/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib 
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin 
export JAVA_HOME JRE_HOME CLASS_PATH PATH
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

### 安装步骤

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

4.一个小例子

Hadoop安装成功后，就可以试试下面这个示例体验一下了。

示例：[用Hadoop统计单词](#docs/hia_01#运行第一个程序（用Hadoop统计单词）)。


### 参考资料

- [安装Hadoop](http://www.powerxing.com/install-hadoop-in-centos/)("安装Java环境"一节不用看，不推荐使用openjdk）
- [Hadoop官方网站](http://hadoop.apache.org/)
- [厦门大学数据库实验室](http://dblab.xmu.edu.cn/)(厦门大学有关大数据的研究资料和教学视频)
- [设置机器间免密登陆](http://blog.csdn.net/dongwuming/article/details/9705595)


