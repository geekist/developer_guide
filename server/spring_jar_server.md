
# 在远程服务器运行Springboot 项目

## 一、将jar上传到服务器。

## 二、安装java sdk。

1、下载tar.gz的压缩包，这里使用官网下载。
进入：
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

勾选接受许可协议后选择对应的压缩包，下载完成后上传的linux服务器上，这里是上传到/tmp 目录下。
也可以通过wget直接下载，注意这里的下载地址是有认证的。

2、下载完成后解压到指定文件下

先创建java文件目录，如果已存在就不用创建

[root@lyh:] # mkdir -p /usr/local/java

解压到java文件目录

[root@lyh:] # tar -vzxf jdk-8u161-linux-x64.tar.gz -C /usr/local/java/

3、添加环境变量，编辑配置文件

[root@lyh:] # vi /etc/profile

在文件最下方或者指定文件添加

export JAVA_HOME=/usr/local/java/jdk1.8.0_161

export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib/

export PATH=$PATH:$JAVA_HOME/bin

保存退出（保存退出的命令是，Shift+:后输入wq回车），然后重新加载配置文件
[root@lyh:] # source /etc/profile

4、最后测试
[root@lyh:] # java -version

可以看到一下信息则表示配置成功

java version “1.8.0_161”

Java™ SE Runtime Environment (build 1.8.0_161-b12)

Java HotSpot™ 64-Bit Server VM (build 25.161-b12, mixed mode


还可以直接用：

```java

yum install java-1.8.0-openjdk

export JAVA_HOME=/usr/java

```
## 3、运行Spring Boot的jar包即可。

java -jar xxx.jar

## 4、后台运行

nohup
使用nohup命令可以解决上述问题，让SpringBoot项目不挂断地在Linux后台运行。
语法：

nohup Command [ Arg … ][ & ]
1
示例：

nohup java -jar xxx.jar &
1
执行上述命令，nohup会把执行结果中的日志默认输出到当前文件夹下面的nohup.out文件中。
也可以手动指定日志输出到哪个文件中。

nohup java -jar xxx.jar > nohup.log  2>&1 & 
1
如果不想输出日志，也可以使用如下命令。

nohup java -jar xxx.jar >/dev/null &
