# 环境安装

**说明**：软件安装使用的系统是CentOS 7.0(Aliyun ECS)，其他Linux环境的安装方法请查阅相关文档，本文不做说明。

## 安装Java

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

对于jdk的安装，通常有下面三种方法，本文采用的是“方法三”。这里会详细介绍第三种安装方法，另外两种方法只做简要介绍。

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


### 方法三：下载rpm包进行安装（本文使用）

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




