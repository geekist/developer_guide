

## 1.Nacos安装环境准备

Nacos 依赖 Java 环境来运行。如果您是从代码开始构建并运行Nacos，还需要为此配置 Maven环境，请确保是在以下版本环境中安装使用:

* 操作系统

64 bit OS，支持 Linux/Unix/Mac/Windows，推荐选用 Linux/Unix/Mac。

* JDK

64 bit JDK 1.8+；下载 & 配置。

* Maven

Maven 3.2.x+；下载 & 配置。


## 2.下载源码或者安装包

你可以通过源码和发行包两种方式来获取 Nacos。


* 下载编译后的压缩包

下载地址： ***https://github.com/alibaba/nacos/releases***

您可以从 最新稳定版本 下载 nacos-server-$version.zip 包。

```
  unzip nacos-server-$version.zip 或者 tar -xvf nacos-server-$version.tar.gz
  cd nacos/bin
```

* 从 Github 上下载源码方式

下载地址： ***https://github.com/alibaba/nacos/releases***

```
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin
```

>版本选择
>您可以在Nacos的release notes及博客中找到每个版本支持的功能的介绍，当前推荐的稳定版本为2.0.3。

## 3.启动服务器

Linux/Unix/Mac

启动命令(standalone代表着单机模式运行，非集群模式):

sh startup.sh -m standalone

如果您使用的是ubuntu系统，或者运行脚本报错提示[[符号找不到，可尝试如下运行：

bash startup.sh -m standalone

Windows
启动命令(standalone代表着单机模式运行，非集群模式):

startup.cmd -m standalone

>如果直接输入startup命令，可能会出现启动失败的问题，需要根据服务器的配置加上参数 standalone 或者 all

## 4.服务注册&发现和配置管理

Nacos配置页面网址：

* IP地址： http://127.0.0.1:8848/nacos

* 用户名： nacos

* 密码： nacos


服务注册
curl -X POST 'http://127.0.0.1:8848/nacos/v1/ns/instance?serviceName=nacos.naming.serviceName&ip=20.18.7.10&port=8080'

服务发现
curl -X GET 'http://127.0.0.1:8848/nacos/v1/ns/instance/list?serviceName=nacos.naming.serviceName'

发布配置
curl -X POST "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=nacos.cfg.dataId&group=test&content=HelloWorld"

获取配置
curl -X GET "http://127.0.0.1:8848/nacos/v1/cs/configs?dataId=nacos.cfg.dataId&group=test"

## 5.关闭服务器

Linux/Unix/Mac
sh shutdown.sh

Windows
shutdown.cmd

或者双击shutdown.cmd运行文件。