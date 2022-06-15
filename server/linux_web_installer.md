
# 一、JDK安装和环境配置

## 检查JDK

### 1、检查JDK是否安装

检查生产环境是否安装了Java

```
java -version
```
如果安装了java，可以看到java的sdk版本和JRE的版本信息

```
[root@geekist ~]# java -version
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
[root@geekist ~]#
```

### 2、查看JDK安装目录

用whereis、which、find等shell工具查找java目录

```
[root@geekist etc]# whereis java
java: /etc/java
[root@geekist etc]# which java
/usr/bin/which: no java in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin)
[root@geekist etc]# find / -name java
/usr/share/bash-completion/completions/java
/etc/java
/etc/pki/java
/etc/pki/ca-trust/extracted/java
```

### 3、检查JDK是否由rpm下载安装或者由压缩包解压安装

通过rpm命令查看是否该安装是由rpm下载安装

```
rpm -qa |grep java
```
如果是由rpm下载安装，则显示如下

```
[root@geekist ~]# rpm -qa |grep java
javapackages-filesystem-5.3.1-7.3.al8.noarch
tzdata-java-2021a-1.1.al8.noarch
java-1.8.0-openjdk-headless-1.8.0.292.b10-0.1.al8.x86_64
java-1.8.0-openjdk-1.8.0.292.b10-0.1.al8.x86_64
```

## 二、删除JDK


### 1、rpm下载安装的JDK的删除

将用 rpm -qa|grep java列出的文件用rpm -e --nodeps 命令逐个删除

```
#rpm -e --nodeps javapackages-filesystem-5.3.1-7.3.al8.noarch
```

删除后用rpm -qa|grep java查看，发现剩下3个，然后依次删除。

```
#rpm -e --nodeps tzdata-java-2021a-1.1.al8.noarch
#rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.292.b10-0.1.al8.x86_64
#rpm -e --nodeps java-1.8.0-openjdk-1.8.0.292.b10-0.1.al8.x86_64
```

全部删除完成后用java -version检查，发现已经无法运行
```
[root@geekist ~]# java -version
-bash: /usr/bin/java: No such file or directory
```

### 2、用JDK压缩包解压安装的卸载：

用whereis命令查找安装目录：

```
where is java



```

然后删除上述目录

```
rm -rf /usr/local/java

```


## 三、安装JDK


### 1、使用yum安装openjdk

查看yum上的java版本

```
yum search  java
yum list java*
```
>上述两种方法查找出来的java版本太多，无法直观选择,直接采用java-1.8.0-openjdk.x86_64 版本。

```
yum install -y java-1.8.0-openjdk.x86_64
```

安装成功后用上述查找方法，可以确认java安装成功。

### 2、使用压缩包安装JDK

到oracle官网：http://www.oracle.com/---资源--下载--archive，找到8u-202版本，最后的免费版本

新建一个 JDK 安装目录
$ mkdir /usr/java
创建一个 /usr/java 目录
$ cp /root/jdk-8u271-linux-x64.tar.gz /usr/java/
将之前下载的 tar 包拷贝到新建的目录下
将 JDK 源码包解压
上一步已经新建了安装目录并且将源码包拷贝到了新目录下，因为是压缩包，因此首先需要对其进行解压操作。

```
$ tar xzf jdk-8u271-linux-x64.tar.gz -C /usr/java
```

## 四、 配置java环境

### 1、配置文件
打开linux配置文件，如果有以前的

```
vi /etc/profile
```

### 2、添加java环境变量

如果有以前安装java的残存配置信息，需要首先删除，然后添加新的配置信息；

```
#set java environment
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib
export JAVA_HOME CLASSPATH PATH
```

```
export JAVA_HOME=/usr/java/jdk1.8.0_271
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
```

### 3、执行profile文件，使配置生效

```
 source  /etc/profile
```

>配置环境变量，是为了有些程序编译时，需要寻找到lib目录和jre目录。


# 二、Git安装和环境配置



# 三、Maven安装和环境配置


# 四、Nginx安装和环境配置