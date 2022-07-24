* [一、JDK安装和环境配置](#一jdk安装和环境配置)
  * [1\.1 查看当前Java环境](#11-查看当前java环境)
  * [1\.2 删除JDK](#12-删除jdk)
    * [1\.2\.1 删除由rpm下载安装的JDK](#121-删除由rpm下载安装的jdk)
    * [1\.2\.2 删除自己安装的JDK](#122-删除自己安装的jdk)
    * [1\.2\.3 清理配置信息](#123-清理配置信息)
  * [1\.3 安装JDK](#13-安装jdk)
    * [1\.3\.1 下载jdk](#131-下载jdk)
    * [1\.3\.2 安装jdk](#132-安装jdk)
    * [1\.3\.3 配置jdk](#133-配置jdk)
* [二、Maven安装和环境配置](#二maven安装和环境配置)
  * [2\.1 查看maven环境](#21-查看maven环境)
  * [2\.2 删除maven](#22-删除maven)
    * [2\.2\.1 删除通过yum方式安装的maven](#221-删除通过yum方式安装的maven)
    * [2\.2\.2 删除通过压缩包方式安装的maven](#222-删除通过压缩包方式安装的maven)
  * [2\.3、安装Maven](#23安装maven)
    * [2\.3\.1 下载maven](#231-下载maven)
    * [2\.3\.2 安装maven](#232-安装maven)
    * [2\.3\.3 配置maven](#233-配置maven)
* [三、Git安装和环境配置](#三git安装和环境配置)
  * [3\.1 检查当前Git环境](#31-检查当前git环境)
    * [3\.1\.1 检查是否安装了git（git \-\-version)](#311-检查是否安装了gitgit---version)
    * [3\.1\.2 检查git是否通过rpm方式安装(rpm \-qa|grep git)](#312-检查git是否通过rpm方式安装rpm--qagrep-git)
    * [3\.1\.3 查找git安装目录](#313-查找git安装目录)
  * [3\.2 卸载Git](#32-卸载git)
    * [3\.2\.1 卸载通过rpm安装的Git](#321-卸载通过rpm安装的git)
    * [3\.2\.2 清除配置信息](#322-清除配置信息)
  * [3\.3 安装Git](#33-安装git)
    * [3\.3\.1 用rpm方式安装Git](#331-用rpm方式安装git)
    * [3\.3\.2 配置Git信息](#332-配置git信息)
* [四、Nginx的安装和环境配置](#四nginx的安装和环境配置)
  * [1、检查](#1检查)
    * [1\.1检查nginx是否正在运行](#11检查nginx是否正在运行)
    * [1\.2通过whereis等方法查看nginx的安装目录](#12通过whereis等方法查看nginx的安装目录)
    * [1\.3检查是否yum安装](#13检查是否yum安装)
  * [2、删除](#2删除)
    * [通过压缩包编译的nginx的卸载](#通过压缩包编译的nginx的卸载)
    * [通过yum安装的nginx的删除](#通过yum安装的nginx的删除)
  * [3、安装](#3安装)
    * [通过下载源码编译的方式安装nginx](#通过下载源码编译的方式安装nginx)
    * [使用yum命令从云应用仓库下载并安装nginx](#使用yum命令从云应用仓库下载并安装nginx)
  * [3、配置](#3配置)
  * [4、常用命令](#4常用命令)
    * [启动nginx](#启动nginx)
    * [带配置文件的启动](#带配置文件的启动)
    * [停止nginx](#停止nginx)
    * [安全停止nginx](#安全停止nginx)
    * [热启动（修改配置文件后重新启动）](#热启动修改配置文件后重新启动)
* [五、Nacos安装和环境配置](#五nacos安装和环境配置)
  * [1、检查](#1检查-1)
    * [1\.1通过进程查看命令查看nacos是否正在运行](#11通过进程查看命令查看nacos是否正在运行)
    * [1\.2通过whereis等命令查看nacos是否安装在本地](#12通过whereis等命令查看nacos是否安装在本地)
  * [2、卸载](#2卸载)
    * [2\.1关闭nacos服务](#21关闭nacos服务)
    * [2\.2删除nacos所在的目录](#22删除nacos所在的目录)
  * [3、安装](#3安装-1)
    * [从 Github 上下载源码方式](#从-github-上下载源码方式)
    * [下载编译后压缩包方式](#下载编译后压缩包方式)
  * [4、配置](#4配置)
  * [5、常用命令](#5常用命令)
    * [5\.1启动(standalone代表着单机模式运行，非集群模式):](#51启动standalone代表着单机模式运行非集群模式)
    * [5\.2关闭](#52关闭)


**Linux环境下web服务器配置，除了git可以不考虑版本，直接通过yum从服务器下载之外，其他如JDK、Maven、Nginx和Nacos都需要下载压缩包安装或编译安装（Nginx需要编译安装）。**

# 一、JDK安装和环境配置

JDK是编译和运行Java服务器程序的依赖项，maven的运行也依赖于JDK。因此，首先需要在服务器安装和配置jdk

**JDK需要从oracle官网下载1.8.202版本，解压安装，不要安装openSDK版本**

>JDK的最后一个免费版本是1.8.202版本。


## 1.1 查看当前Java环境

* 检查生产环境是否安装了Java 

**java -version**

可以看到java的sdk版本和JRE的版本信息

```shell
[root@geekist ~]# java -version
java version "1.8.0_202"
Java(TM) SE Runtime Environment (build 1.8.0_202-b08)
Java HotSpot(TM) 64-Bit Server VM (build 25.202-b08, mixed mode)
```

如果安装的opensdk版本，

```shell
[root@geekist ~]# java -version
openjdk version "1.8.0_292"
OpenJDK Runtime Environment (build 1.8.0_292-b10)
OpenJDK 64-Bit Server VM (build 25.292-b10, mixed mode)
[root@geekist ~]#
```

一般linux系统会默认通过rpm安装opensdk。

rpm是linux系统中软件包管理的底层通用工具。通过rpm命令查看是否该安装是由rpm下载安装.rpm -qa 显示安装到系统中的所有软件包列表：

* 检查java是否通过rpm软件包工具安装 

如果已经安装了java，可以通过rpm软件包功能检查是否由rpm安装

**rpm -qa |grep java**

如果是由rpm下载安装，则显示如下

```shell
[root@geekist ~]# rpm -qa |grep java
javapackages-filesystem-5.3.1-7.3.al8.noarch
tzdata-java-2021a-1.1.al8.noarch
java-1.8.0-openjdk-headless-1.8.0.292.b10-0.1.al8.x86_64
java-1.8.0-openjdk-1.8.0.292.b10-0.1.al8.x86_64
```

## 1.2 删除JDK

安装JDK之前，需要检查服务器上是否已经存在Java的环境，如果Java环境和要求不符合，需要首先删除已经安装的Java开发包。

### 1.2.1 删除由rpm下载安装的JDK

将用 rpm -qa|grep java列出的文件用rpm -e --nodeps 命令逐个删除

**rpm -e --nodeps javapackages-filesystem-5.3.1-7.3.al8.noarch**

删除后用rpm -qa|grep java查看，发现还剩下3个，然后依次删除。

```shell
#rpm -e --nodeps tzdata-java-2021a-1.1.al8.noarch
#rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.292.b10-0.1.al8.x86_64
#rpm -e --nodeps java-1.8.0-openjdk-1.8.0.292.b10-0.1.al8.x86_64
```

也可以用上层的yum命令来删除

**yum remove java**

全部删除完成后用java -version检查，发现已经无法运行

### 1.2.2 删除自己安装的JDK 

* step1: 首先用whereis、which、find等shell工具查找java目录

**whereis java 搜索二进制文件**

whereis 搜索二进制文件

```
[root@geekist ~]# whereis java
java: /usr/local/java /usr/local/java/jdk1.8.0_202/bin/java
``` 

**which 搜索bin目录下文件**

```shell
[root@geekist ~]# which java
/usr/local/java/jdk1.8.0_202/bin/java
```

**find 搜索所有文件**

```shell
[root@geekist ~]# find / -iname java
/root/.m2/repository/net/java
/usr/share/bash-completion/completions/java
/usr/local/java
/usr/local/java/jdk1.8.0_202/bin/java
/usr/local/java/jdk1.8.0_202/jre/bin/java
/etc/pki/java
/etc/pki/ca-trust/extracted/java
/var/yuya/yychildren/yychildren-common/src/main/java
/var/yuya/yychildren/yychildren-generator/src/test/java
/var/yuya/yychildren/yychildren-generator/src/main/java
/var/yuya/yychildren/yychildren-console/src/main/java
/var/yuya/yychildren/yychildren/yychildren-console/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-task/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-core/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-common/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-parent/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-generator/src/test/java
/var/yuya/yychildren_server_springcloud/yychildren-generator/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-console/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-gateway/src/main/java
/var/yuya/yychildren_server_springcloud/yychildren-teacher/src/main/java
```
可以看到java安装在/usr/local/java目录下

* step2: 用rm 命名删除已安装的java

**rm -rf /usr/local/java**

用rm命令删除已经安装的JDK

```shell
rm -rf /usr/local/java
```

### 1.2.3 清理配置信息

如果配置文件/etc/profile中配置了环境变量，也需要删除。

```shell
vim /etc/profile

删除下面的jdk配置
 78 #JDK环境变量配置：
 79 export JAVA_HOME=/usr/local/java/jdk1.8.0_202
 80 export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
 81 export PATH=$PATH:$JAVA_HOME/bin
 ```

## 1.3 安装JDK

### 1.3.1 下载jdk

在oracle官网下载jdk_8u_202版本

到oracle官网：http://www.oracle.com/ ---资源--下载--JDK--archive，找到8u-202版本，最后的免费版本


### 1.3.2 安装jdk

**mkdir /usr/local/java**

新建一个 JDK 安装目录java

```
mkdir /usr/local/java
cd /usr/local/java
```

将之前下载的 tar 包拷贝到新建的目录下
>拷贝可以用各自的linux桌面工具。

**tar zxvf jdk-8u202-linux-x64.tar.gz** 

将 JDK 源码包解压
```
$ tar zxvf jdk-8u202-linux-x64.tar.gz 
```

### 1.3.3 配置jdk

文件安装完成，下面进行配置。

**vim /etc/profile**

打开linux配置文件，如果有以前的配置，可以先删除。

```shell
vim /etc/profile
```

如果有以前安装java的残存配置信息，需要首先删除，然后添加新的配置信息；

JDK的配置

```shell
export JAVA_HOME=/usr/local/java/jdk1.8.0_202
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
```

执行profile文件，使配置生效

**source  /etc/profile**

```shell
 source  /etc/profile
```

至此，JDK安装完成。

>使用yum安装openjdk
>
>查看yum上的java版本
>
>```
>yum search  java
>yum list java*
>```
>上述两种方法查找出来的java版本太多，无法直观选择,直接采用java-1.8.0-openjdk.x86_64 版本。
>
>```
>yum install -y java-1.8.0-openjdk.x86_64
>```
>
>安装成功后用上述查找方法，可以确认java安装成功。
>
>yum 安装openjdk，经测试直接安装在/usr目录下。手动删除需要逐个删除java目录。
>
>打开linux配置文件，如果有以前的配置，可以先删除。
>
>```shell
>vim /etc/profile
>```
>
>如果有以前安装java的残存配置信息，需要首先删除，然后添加新的配置信息；
>
>openJDK的配置
>```shell
>#set java environment
>JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
>PATH=$PATH:$JAVA_HOME/bin
>CLASSPATH=.:$JAVA_HOME/lib
>export JAVA_HOME CLASSPATH PATH
>```
>
>执行profile文件，使配置生效
>
>**source  /etc/profile**
>
>```shell
> source  /etc/profile
>```
>至此，openSDK安装完成。

配置环境变量，是为了有些程序编译时，需要寻找到lib目录和jre目录。

# 二、Maven安装和环境配置


Maven用来编译Java Web服务器程序。

**Maven必须通过下载安装包的方式安装，通过yum方式安装会默认安装openjdk依赖**


## 2.1 查看maven环境


* 查看系统是否安装了maven

用mvn -v命令可以查看maven是否安装

**mvn -v**

```shell
[root@geekist yychildren]# mvn -v
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/apache-maven-3.8.6
Java version: 1.8.0_332, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.23-5.al8.x86_64", arch: "amd64", family: "unix"
```

* 查看Maven是否由rpm下载安装

**rpm -qa |grep maven**

通过rpm命令查看是否该安装是由rpm下载安装

如果是由rpm下载安装，则显示如下
```
[root@geekist yychildren]# rpm -qa|grep maven
maven-resolver-1.4.1-3.1.al8.noarch
maven-shared-utils-3.2.1-0.1.1.al8.noarch
maven-wagon-3.3.4-2.1.al8.noarch
maven-lib-3.6.2-6.1.al8.noarch
maven-3.6.2-6.1.al8.noarch
maven-openjdk11-3.6.2-6.1.al8.noarch
```
## 2.2 删除maven

### 2.2.1 删除通过yum方式安装的maven

**yum remove maven**

```
yum remove maven 
```
或者通过rpm -e nodeps方式卸载。

**rpm -e --nodeps maven-resolver-1.4.1-3.1.al8.noarch**

逐个删除maven程序。

最后可以删除/etc/profile配置文件


### 2.2.2 删除通过压缩包方式安装的maven


* 查看maven的安装目录

**mvn -v**

首先查找maven的安装路径，可以通过mvn -v或其他命令

```
[root@geekist yychildren]# mvn -v
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/apache-maven-3.8.6
Java version: 1.8.0_332, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.23-5.al8.x86_64", arch: "amd64", family: "unix"
```

也可以用whereis、which、find等shell工具查找java目录
```
[root@geekist yychildren]# whereis mvn
mvn: /usr/local/apache-maven-3.8.6/bin/mvn /usr/local/apache-maven-3.8.6/bin/mvn.cmd
[root@geekist yychildren]# which mvn
/usr/local/apache-maven-3.8.6/bin/mvn
[root@geekist yychildren]# find / -name mvn
/usr/local/apache-maven-3.8.6/bin/mvn
[root@geekist yychildren]# find / -name maven
/root/.m2/repository/org/apache/maven
/root/.m2/repository/org/apache/maven/maven
```

* 然后删除maven所在目录
```
rm -rf /usr/local/apach-maven-3.8.6
```

### 2.2.3 清理配置信息

最后可以删除/etc/profile配置文件

```shell
 #Maven环境变量配置：
export MAVEN_HOME=/usr/local/maven/apache-maven-3.8.6
export PATH=$PATH:$MAVEN_HOME/bin
```

## 2.3、安装Maven

### 2.3.1 下载maven

前往https://maven.apache.org/download.cgi下载最新版的Maven程序


新建一个 JDK 安装目录java
```
mkdir /usr/local/maven
cd /usr/local/maven
```

将之前下载的 tar 包拷贝到新建的目录下
>拷贝可以用各自的linux桌面工具。

### 2.3.2 安装maven

解压maven到当前目录或指定目录
```
tar zxvf apache-maven-3.8.6-bin.tar.gz
```

### 2.3.3 配置maven

```shell
vi /etc/profile
```
在打开的文件中追加配置
```shell
export MAVEN_HOME=/usr/local/maven/apache-maven-3.8.6
export PATH=$PATH:$MAVEN_HOME/bin
```
重新加载配置文件
```
source /etc/profile
```

>用yum方式安装Maven
>```
>yum -y install maven 
>```
>安装成功后，查看安装路径：
>
>```
>[root@geekist /]# rpm -qa |grep maven
>maven-lib-3.6.2-6.1.al8.noarch
>maven-3.6.2-6.1.al8.noarch
>maven-resolver-1.4.1-3.1.al8.noarch
>maven-openjdk11-3.6.2-6.1.al8.noarch
>maven-shared-utils-3.2.1-0.1.1.al8.noarch
>maven-wagon-3.3.4-2.1.al8.noarch
>```

# 三、Git安装和环境配置

 Git用来从代码服务器获取代码。Git工具可以通过yum下载。

 本文只讨论yum方式的删除和安装

## 3.1 检查当前Git环境

### 3.1.1 检查是否安装了git（git --version)

```shell
[root@geekist java]# git --version
git version 2.27.0
```

### 3.1.2 检查git是否通过rpm方式安装(rpm -qa|grep git)

**rpm -qa|grep git**

可以看到：

`git-core-doc-2.27.0-1.1.al8.noarch`


```shell
[root@geekist java]# rpm -qa|grep git
git-core-2.27.0-1.1.al8.x86_64
audit-libs-3.0-0.17.20191104git1c2f876.1.al8.x86_64
plymouth-scripts-0.9.4-7.20200615git1e36e30.1.al8.x86_64
net-tools-2.0-0.52.20160912git.1.al8.x86_64
linux-firmware-20200619-99.git3890db36.1.al8.noarch
crontabs-1.11-16.20150630git.1.al8.noarch
dracut-squash-049-95.git20200804.1.al8.x86_64
plymouth-0.9.4-7.20200615git1e36e30.1.al8.x86_64
audit-3.0-0.17.20191104git1c2f876.1.al8.x86_64
lm_sensors-libs-3.4.0-21.20180522git70f7e08.1.al8.x86_64
git-core-doc-2.27.0-1.1.al8.noarch
git-2.27.0-1.1.al8.x86_64
crypto-policies-scripts-20200713-1.git51d1222.1.al8.noarch
crypto-policies-20200713-1.git51d1222.1.al8.noarch
dracut-049-95.git20200804.1.al8.x86_64
dracut-network-049-95.git20200804.1.al8.x86_64
dracut-config-rescue-049-95.git20200804.1.al8.x86_64
libnsl2-1.2.0-2.20180605git4a062cf.2.al8.x86_64
plymouth-core-libs-0.9.4-7.20200615git1e36e30.1.al8.x86_64
```

### 3.1.3 查找git安装目录
```
[root@iZbp19n36uysranoj3k2x5Z etc]# whereis git
git: /usr/bin/git /usr/share/man/man1/git.1.gz

[root@iZbp19n36uysranoj3k2x5Z etc]# which git
/usr/bin/git

[root@iZbp19n36uysranoj3k2x5Z etc]# find / -name git
/var/lib/selinux/targeted/active/modules/100/git
/usr/bin/git
/usr/share/doc/git
/usr/share/doc/git/contrib/mw-to-git/bin-wrapper/git
/usr/share/bash-completion/completions/git
/usr/share/emacs/site-lisp/git
/usr/share/selinux/targeted/default/active/modules/100/git
/usr/libexec/git-core/git
```

## 3.2 卸载Git


### 3.2.1 卸载通过rpm安装的Git

**yum remove git**

通过rpm或yum安装的git，可以通过yum方式卸载git
```
yum remove git
```

### 3.2.2 清除配置信息

如果有配置保存在系统的配置文件里，同时需要删除。git的配置文件有三层：分别是：全局配置、当前用户、当前项目配置。一般用当前用户的配置。
`~/.gitconfig`

* /etc/gitconfig 文件（全局配置）

* ~/.gitconfig 或 ~/.config/git/config 文件：（当前用户）

* 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：

一般在~/.gitconfig文件中可以看到：

```s
[user]
    email = yuyaapp@163.com
    name = 13681986288

[credential]
    helper = store
```
删除原来的配置即可。

## 3.3 安装Git


### 3.3.1 用rpm方式安装Git

**yum install -y git**

**yum install -y git**

用yum 安装git
```
yum install -y git
```

### 3.3.2 配置Git信息

* 配置保存用户名和密码，不用每次都输入

查找git的配置文件：

>/etc/gitconfig 文件（全局配置）
>
> ~/.gitconfig 或 ~/.config/git/config 文件：（当前用户）
>
> 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：
>
>一般在~/.gitconfig文件中可以看到该配置文件：

添加如下配置即可

```
[user]
    email = yuyaapp@163.com
    name = 13681986288
[credential]
    helper = store
```

# 四、Nginx的安装和环境配置

* Nginx用来做WebServer服务器，实现反向代理和负载均衡等功能。（`需要编译安装，这样可以配置websocket、ssl等模块`）

## 1、检查

### 1.1检查nginx是否正在运行

```
ps -ef|grep nginx
```

### 1.2通过whereis等方法查看nginx的安装目录
```
[root@geekist local]# whereis nginx
nginx: /usr/local/nginx

[root@geekist local]# which nginx
/usr/bin/which: no nginx in (/usr/local/jdk1.8.0_202/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre/bin:/usr/local/apache-maven-3.8.6/bin:/root/bin:/usr/local/apache-maven-3.8.6/bin)

[root@geekist local]# find / -name nginx
/usr/local/nginx
/usr/local/nginx/sbin/nginx
/usr/nginx_installer/nginx-1.22.0/objs/nginx
[root@geekist local]#
```

### 1.3检查是否yum安装

```
rpm -qa|grep nginx
```

## 2、删除

### 通过压缩包编译的nginx的卸载

* 1、查看nginx是否活动
```
ps -ef|grep nginx
```
* 2、如果活动进程，先停止nginx

```
./nginx -s stop
```

* 3、查找nginx的所有相关目录
```
[root@geekist local]# whereis nginx
nginx: /usr/local/nginx
[root@geekist local]# which nginx
/usr/bin/which: no nginx in (/usr/local/jdk1.8.0_202/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre/bin:/usr/local/apache-maven-3.8.6/bin:/root/bin:/usr/local/apache-maven-3.8.6/bin)
[root@geekist local]# find / -name nginx
/usr/local/nginx
/usr/local/nginx/sbin/nginx
/usr/nginx_installer/nginx-1.22.0/objs/nginx
[root@geekist local]#
```

* 4、对找到的目录删除相关文件

```
[root@localhost ~]# rm -rf /usr/local/nginx
[root@localhost ~]# rm -rf /usr/local/nginx/sbin/nginx
[root@localhost ~]# rm -rf /var/spool/mail/nginx
```
### 通过yum安装的nginx的删除
```
[root@localhost ~]# yum remove nginx
Loaded plugins: fastestmirror
No Match for argument: nginx
No Packages marked for removal
```
或者使用rpm -e nodeps 卸载；

## 3、安装

### 通过下载源码编译的方式安装nginx

* step1:在Nginx官网：http://nginx.org/en/download.html 下载nginx，最新版本是1.20.2

网页上提供了Nginx 服务器三种版本的下载，分别是：

>开发版（Mainline version）
>
>稳定版本（Stable version
>
>过期版本（Legacy versions）

>页面上下载部分各链接的具体含义：

>“CHANGES-x.xx”链接，记录的是对应版本的功能变更日志。包括新增功能、功能的优化和功能缺陷的修复等。

>“nginx-x.x.x”链接，是 Nginx服务器的 Linux版本下载地址。

>“nginx/Windows-x.x.x”链接，是 Nginx 服务器的Windows版本下载地址。

>“pgp”链接，记录的是提供下载的版本使用PGP加密自由软件GnuPG计算后的签名。PGP可以理解为Pretty Good Privacy。这些数据可以用于下载文件的验证。

* step2:使用ftp工具等，将nginx上传到linux。

```
cd /usr/local

mkdir nginx

cd nginx
# 上传nginx到该目录下
```

* step3:安装GCC与dev库

```
GCC编译器:yum install gcc gcc-c++

正则表达式PCRE库:yum install -y pcre pcre-devel

zlib压缩库:yum install -y zlib zlib-devel

OpenSSL开发库:yum install -y openssl openssl-devel
```

可以一键安装：
```
yum -y install gcc zlib zlib-devel pcre pcre-devel openssl openssl-devel

```

* step4:编译Nginx

```
//进入nginx目录
cd /usr/local/nginx
//进入目录
cd nginx-1.13.7
//执行命令
./configure
//执行make命令
make
//执行make install命令
make install
```
至此，nginx安装成功。

>nignx的安装命令，会将nginx编译后的lib和bin都放置在/usr/local/nginx目录下。所以，安装和编辑的目录不限。
>
>可以配置nginx的环境变量，将nginx的路径加入到/etc/profile文件中，师nginx可以在任意地方启动，也可以不配置，运行时加上完整的目录。

### 使用yum命令从云应用仓库下载并安装nginx

```
$ sudo yum -y install nginx   # 安装 nginx
```

如果没有配置EPEL仓库，可以先运行下面的命令来完成安装：
```
sudo yum install epel-release

```

安装成功后，默认的网站目录为： /usr/share/nginx/html

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

此时，nginx已经被添加到了环境变量中，可以直接在任意路径下使用。


## 3、配置

>Nginx的配置文件通过config目录下的nginx.conf来进行配置，详情参见nginx的配置文档。

## 4、常用命令

### 启动nginx

```
执行启动命令：
./nginx                       //启动
```

### 带配置文件的启动

```
./nginx -c conf/nginx.conf
```

### 停止nginx


```
./nginx -s stop
```

### 安全停止nginx

```
./nginx -s quit                
```

### 热启动（修改配置文件后重新启动）
```
./nginx -t
```

```
./nginx -s reload 
```

# 五、Nacos安装和环境配置

系统采用spring-cloud-alibaba实现微服务管理。

nacos快速安装文档：https://nacos.io/zh-cn/docs/quick-start.html

## 1、检查

### 1.1通过进程查看命令查看nacos是否正在运行

```
[root@iZbp19n36uysranoj3k2x5Z ~]# ps -ef|grep nacos
root      765081       1  0 May07 ?        02:42:17 /usr/java/jdk1.8/bin/java -Xms512m -Xmx512m -Xmn256m -Dnacos.standalone=true -Dnacos.member.list= -Djava.ext.dirs=/usr/java/jdk1.8/jre/lib/ext:/usr/java/jdk1.8/lib/ext -Xloggc:/usr/local/nacos/logs/nacos_gc.log -verbose:gc -XX:+PrintGCDetails -XX:+PrintGCDateStamps -XX:+PrintGCTimeStamps -XX:+UseGCLogFileRotation -XX:NumberOfGCLogFiles=10 -XX:GCLogFileSize=100M -Dloader.path=/usr/local/nacos/plugins/health,/usr/local/nacos/plugins/cmdb -Dnacos.home=/usr/local/nacos -jar /usr/local/nacos/target/nacos-server.jar --spring.config.additional-location=file:/usr/local/nacos/conf/ --logging.config=/usr/local/nacos/conf/nacos-logback.xml --server.max-http-header-size=524288 nacos.nacos
root     3681989 3681899  0 10:29 pts/0    00:00:00 grep --color=auto nacos
```
### 1.2通过whereis等命令查看nacos是否安装在本地

```
[root@iZbp19n36uysranoj3k2x5Z ~]# whereis nacos
nacos: /usr/local/nacos

[root@iZbp19n36uysranoj3k2x5Z ~]# which nacos
/usr/bin/which: no nacos in (/usr/local/mysql/mysql-5.6/bin:/usr/java/jdk1.8/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/local/apache-maven-3.6.3/bin:/root/bin)

[root@iZbp19n36uysranoj3k2x5Z ~]# find / -name nacos
/root/.m2/repository/com/alibaba/nacos
/root/logs/nacos
/root/nacos
/usr/local/nacos
/usr/local/nacos/bin/work/Tomcat/localhost/nacos
/usr/local/nacos/work/Tomcat/localhost/nacos
```
## 2、卸载

### 2.1关闭nacos服务

```
shutdown.sh
```

### 2.2删除nacos所在的目录

```
rm -rf /usr/local/nacos
```

## 3、安装

### 从 Github 上下载源码方式
```
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin
```

### 下载编译后压缩包方式

nacos 下载地址：https://github.com/alibaba/nacos/releases

从最新稳定版本下载nacos-server-$version.zip 包。(当前官方最新文档为2.0.3)

```
unzip nacos-server-$version.zip 或者 tar -xvf nacos-server-$version.tar.gz
cd nacos/bin
```
## 4、配置

>当前的nacos配置，不需要通过服务器端配置文件进行特殊配置。

## 5、常用命令

### 5.1启动(standalone代表着单机模式运行，非集群模式):
```
startup.sh -m standalone

如果您使用的是ubuntu系统，或者运行脚本报错提示[[符号找不到，可尝试如下运行：
bash startup.sh -m standalone
```

### 5.2关闭
```
shutdown.sh
```