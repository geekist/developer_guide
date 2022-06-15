
在linux服务器上运行java web服务器，需要安装JDK、Git、Maven和Nginx。
其中：

* JDK是编译和运行Java服务器程序的依赖项，maven的运行也依赖于JDK。`JDK需要从oracle官网下载1.8.202版本，解压安装，不要安装openSDK版本`

* Git用来从代码服务器获取代码。`Git工具可以通过yum下载`

* Maven用来编译Java Web服务器程序。`Maven需要从官网下载，解压安装，不能通过yum安装，因为会默认安装opensdk`

* Nginx用来做WebServer服务器，实现反向代理和负载均衡等功能。

# 一、JDK安装和环境配置

## ***安装JDK前的检查***
安装JDK之前，需要检查服务器上是否已经存在Java的环境，如果Java环境和要求不符合，需要首先删除已经安装的Java，`如果已经预装了OpenSDK，需要先删除掉`。JDK的最后一个免费版本是1.8.202版本。

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
## ***删除已经安装的JDK***

### * 通过rpm下载安装的JDK的删除
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
或者可以用yum命令：
```
yum remove maven
```
全部删除完成后用java -version检查，发现已经无法运行
```
[root@geekist ~]# java -version
-bash: /usr/bin/java: No such file or directory
```
### *通过JDK压缩包解压安装的卸载：
用whereis命令查找安装目录：
```
where is java
```

然后删除上述目录
```
rm -rf /usr/local/java
```

## ***安装JDK***
### * 使用yum安装openjdk
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

### * 使用压缩包安装JDK

到oracle官网：http://www.oracle.com/---资源--下载--archive，找到8u-202版本，最后的免费版本

新建一个 JDK 安装目录
cd /usr/local

将之前下载的 tar 包拷贝到新建的目录下

将 JDK 源码包解压
```
$ tar zxvf jdk-8u202-linux-x64.tar.gz 
```

## ***配置java环境***

### 1、配置文件
打开linux配置文件，如果有以前的

```
vi /etc/profile
```

### 2、添加java环境变量

如果有以前安装java的残存配置信息，需要首先删除，然后添加新的配置信息；

openJDK的配置
```
#set java environment
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
PATH=$PATH:$JAVA_HOME/bin
CLASSPATH=.:$JAVA_HOME/lib
export JAVA_HOME CLASSPATH PATH
```

JDK的配置
```
export JAVA_HOME=/usr/local/jdk1.8.0_202
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib
export PATH=$JAVA_HOME/bin:$PATH
```

### 3、执行profile文件，使配置生效

```
 source  /etc/profile
```
>配置环境变量，是为了有些程序编译时，需要寻找到lib目录和jre目录。


# 二、Git安装和环境配置


## 一、安装和卸载Git


### windows下如何安装和卸载git

如果安装了github，sourcetree等代码管理工具，则可以在响应的工具目录下找到git的可执行命令，使用即可；卸载则同样可以卸载相应的源代码管理工具即可；

可以直接从git的官方网站下载：https://git-scm.com/downloads，然后安装即可，卸载时也直接卸载安装包即可。

### linux下如何安装和卸载git
以centOS系列的git安装为例：
首先，查看系统中是否安装了git
```
whereis git
如果出现显示
git： 说明系统中没有安装git
```
用yum 安装git
```
yum install -y git
```
安装完成后可以用whereis 、which 或 git version、ps等命令检查git是否安装
```
whereis git
which git
git version
git --version
ps -ef|grep git
root     2390148 2387451  0 11:02 pts/1    00:00:00 grep --color=auto git
```

卸载git
```
yum remove git
```

## 二、配置git

### git配置
既然已经在系统上安装了 Git，你会想要做几件事来定制你的 Git 环境。 每台计算机上只需要配置一次，程序升级时会保留配置信息。 你可以在任何时候再次通过运行命令来修改它们。

Git 自带一个 git config 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

* /etc/gitconfig 文件: （实践中centos环境下没有该配置文件）

包含系统上每一个用户及他们仓库的通用配置。 如果在执行 git config 时带上 --system 选项，那么它就会读写该文件中的配置变量。 （由于它是系统配置文件，因此你需要管理员或超级用户权限来修改它。）

* ~/.gitconfig 或 ~/.config/git/config 文件：

实践中没有.config/git/config文件。

发现~/.gitconfig文件

只针对当前用户。 你可以传递 --global 选项让 Git 读写此文件，这会对你系统上 所有 的仓库生效。

* 当前使用仓库的 Git 目录中的 config 文件（即 .git/config）：

针对该仓库。 你可以传递 --local 选项让 Git 强制读写此文件，虽然默认情况下用的就是它。。 （当然，你需要进入某个 Git 仓库中才能让该选项生效。）

优先级：

每一个级别会覆盖上一级别的配置，所以 .git/config 的配置变量会覆盖 /etc/gitconfig 中的配置变量。


### 修改git配置信息

例子：

安装完 Git 之后，要做的第一件事就是设置你的用户名和邮件地址。 这一点很重要，因为每一个 Git 提交都会使用这些信息，它们会写入到你的每一次提交中，不可更改：

```
$ git config --global user.name "Daniel Yang"
$ git config --global user.email johndoe@example.com
```

### 查看git配置信息

如果想要检查你的配置，可以使用 git config --list 命令来列出所有 Git 当时能找到的配置。

```
$ git config --list

user.name=John Doe
user.email=johndoe@example.com
color.status=auto
color.branch=auto
color.interactive=auto
color.diff=auto
...

```

你可能会看到重复的变量名，因为 Git 会从不同的文件中读取同一个配置（例如：/etc/gitconfig 与 ~/.gitconfig）。 这种情况下，Git 会使用它找到的每一个变量的最后一个配置。

你可以通过输入 git config <key>： 来检查 Git 的某一项配置

```
$ git config user.name
John Doe
```
### 配置保存用户名和密码，不用每次都输入

在C盘用户当前用户的目录下找到.gitconfig文件，用记事本打开，或者借助其他工具打开进行编辑。
在文件中可以看到：
```
[user]
name = user
email = 123456789@163.com
```
在这个文件内容后面添加：
```
[credential]
helper = store
```
保存如下：
```
[credential]
        helper = store
[user]
        email = you@example.com
        name = 13681986288
```
设置credential后，下次运行，只需要输入一遍，就可以再也不用输入用户名和密码了。


# Maven安装和环境配置

## 一、检查Maven
### 1、检查Maven是否安装

用mvn -v命令可以查看maven是否安装
```
[root@geekist yychildren]# mvn -v
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/apache-maven-3.8.6
Java version: 1.8.0_332, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.23-5.al8.x86_64", arch: "amd64", family: "unix"
```

### 2、查看Maven安装目录
用whereis、which、find等shell工具查找java目录
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
### 3、检查Maven是否由rpm下载安装或者由压缩包解压安装
通过rpm命令查看是否该安装是由rpm下载安装
```
rpm -qa |grep maven
```
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
## 二、卸载Maven

### 1、删除通过yum方式安装的maven
```
yum remove maven 
```

### 2、删除通过压缩包方式安装的maven

首先查找maven的安装路径，可以通过mvn -v或其他命令
```
[root@geekist yychildren]# mvn -v
Apache Maven 3.8.6 (84538c9988a25aec085021c365c560670ad80f63)
Maven home: /usr/local/apache-maven-3.8.6
Java version: 1.8.0_332, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.332.b09-1.al8.x86_64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.10.23-5.al8.x86_64", arch: "amd64", family: "unix"
```

然后删除maven所在目录
```
rm -rf /usr/local/apach-maven-3.8.6
```
最后可以删除/etc/profile配置文件

## 三、安装Maven

### 1、用yum方式安装Maven
```
yum -y install maven 
```
安装成功后，查看安装路径：

```
[root@geekist /]# rpm -qa |grep maven
maven-lib-3.6.2-6.1.al8.noarch
maven-3.6.2-6.1.al8.noarch
maven-resolver-1.4.1-3.1.al8.noarch
maven-openjdk11-3.6.2-6.1.al8.noarch
maven-shared-utils-3.2.1-0.1.1.al8.noarch
maven-wagon-3.3.4-2.1.al8.noarch
```
***安装Maven  --尽量不要用yum的方式安装maven，因为这样会重新安装JDK***

### 2、用压缩包的方式安装maven

前往https://maven.apache.org/download.cgi下载最新版的Maven程序

将下载的压缩包上传到服务器的/usr/local目录下

解压maven到当前目录或指定目录
```
tar zxvf apache-maven-3.8.6-bin.tar.gz
```

### 3、配置环境变量
```
vi /etc/profile
```
在打开的文件中追加配置
```
export MAVEN_HOME=/usr/local/apache-maven-3.8.3
export PATH=$PATH:$MAVEN_HOME/bin
```
重新加载配置文件
```
source /etc/profile
```

# 二、Nginx的安装和卸载


可以在Nginx官网：http://nginx.org/en/download.html 下载nginx，最新版本是1.20.2

网页上提供了Nginx 服务器三种版本的下载，分别是：

* 开发版（Mainline version）

* 稳定版本（Stable version

* 过期版本（Legacy versions）

页面上下载部分各链接的具体含义：

“CHANGES-x.xx”链接，记录的是对应版本的功能变更日志。包括新增功能、功能的优化和功能缺陷的修复等。

“nginx-x.x.x”链接，是 Nginx服务器的 Linux版本下载地址。


“pgp”链接，记录的是提供下载的版本使用PGP加密自由软件GnuPG计算后的签名。PGP可以理解为Pretty Good Privacy。这些数据可以用于下载文件的验证。

“nginx/Windows-x.x.x”链接，是 Nginx 服务器的Windows版本下载地址。


## window环境下Nginx的安装和卸载

nginx在windows下免安装，直接运行即可

## linux环境下Nginx的安装和卸载

### 彻底卸载nginx

* step1：查看是否有正在运行的nginx进程,如果有，则停滞nginx

```
ps -ef|grep nginx

```
停止nginx 服务,在nginx的目录下执行

```
./nginx -s stop
```

* step2:查找nginx的所有相关目录

```
which nginx
```

```
[root@localhost ~]# whereis nginx
nginx: /usr/local/nginx
```

```
[root@localhost ~]# find / -name nginx
/var/spool/mail/nginx
/usr/local/nginx
/usr/local/nginx/sbin/nginx

```

* step3:对找到的目录删除相关文件

```
[root@localhost ~]# rm -rf /usr/local/nginx
[root@localhost ~]# rm -rf /usr/local/nginx/sbin/nginx
[root@localhost ~]# rm -rf /var/spool/mail/nginx

```

* step4:使用yum清理

```
[root@localhost ~]# yum remove nginx
Loaded plugins: fastestmirror
No Match for argument: nginx
No Packages marked for removal
```

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

### 从源代码编译安装nginx

* step1：首先从nginx官网下载压缩包，nginx-1.22.0.tar.gz，然后使用ftp工具等，将nginx上传到linux。

```
cd /usr/local

mkdir nginx

cd nginx
```

* step2：安装GCC与dev库

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

* step3:编译Nginx

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
至此，nginx安装成功
