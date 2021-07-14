# 在本地运行Spring boot 项目

对于传统的Web项目，可能大部分都要部署到web容器中，如Tomcat。Spring Boot提供了一种超级简单的部署方式，就是直接将应用打成jar包，在生产上只需要执行java -jar就可以运行了。


一般情况下，如果我们的应用只是作为一个服务、工具类、API的形式存在，那么我们一般将其打包成jar包。而如果我们的应用是一个Web应用，都是打成war包，进行发布，同时如果我们的服务器是Tomcat等轻量级服务器，一般都打成war包进行发布

## 配置pom.xml将packaging设置为jar
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<!--从Spring Boot 继承默认配置-->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.1</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>


	<groupId>com.ytech</groupId>
	<artifactId>hellospring</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>hellospring</name>
	<packaging>jar</packaging>

	<description>Demo project for Spring Boot</description>
</project>
```

## 配置maven插件，打包jar--实际测试中该插件无法下载
```xml
<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

```

## 进入pom.xml所在的目录，运行mvn clean package -Dmaven.test.skip=true命令打包。
实测中因为该插件无法下载，导致编译失败

![](https://github.com/geekist/developer_guide/blob/main/server/assets/spring-jar.png)

## 运行--进入target目录，运行java -jar xxx.jar，则运行起来了。
linux 环境下：

通过 java -jar 命令运行该 jar 包，例如：
java -jar love-0.0.1-SNAPSHOT.jar

这个问题是因为配置环境变量的问题，把环境配置刷新即可
source /etc/profile
如果服务器上没有部署JDK则参考文章：Linux上安装JDK以及配置环境（服务器）

项目启动成功后，按 Ctrl + Z 使得进程后台运行
如果项目想要更新需要关闭后台正在运行的进程
#列出后台进程
ps -ef | grep java
#杀死进程，-9：无条件终止进程，xxxx：要关闭进程的pid
kill -9 xxxx


## 停止spring boot

windows下关闭spring boot 的jar，直接关闭dos窗口即可。或者到任务管理器中去关闭。

使用 spring-boot-starter-actuator 提供的 /shutdown 进行优雅停机


@echo off
curl -X POST http://127.0.0.1:18881/eurekaManagement/shutdown
echo
echo backend-eureka-server-0.0.1-SNAPSHOT.jar
pause