
## 一个最简单的pom.xml文件

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.4</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.ytech</groupId>
	<artifactId>yuya</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>yuya</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>11</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

## 1、版本模型

```xml

<modelVersion>4.0.0</modelVersion>

```
## 2、项目信息

```xml
<!-- 项目信息 -->
    <groupId>demo</groupId><!-- 项目唯一标识 -->
    <artifactId>springboot</artifactId><!-- 项目名 -->
    <version>0.0.1-SNAPSHOT</version><!-- 版本 -->
    <packaging>jar</packaging><!-- 打包方式 （pom,war,jar） -->
 
    <name>springboot</name><!-- 项目的名称， Maven 产生的文档用 -->
    <description>Demo project for Spring Boot</description><!-- 项目的描述, Maven 产生的文档用 -->
```

## 3、Parent节点

要成为一个spring boot项目，首先就必须在pom.xml中继承spring-boot-starter-parent，同时指定其版本。

```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
	</parent>
```

## 4、属性设置（环境参数）

在普通maven项目中，需要在pom.xml中配置插件来修改jdk版本，utf-8编码等环境参数，在spring boot中则更加简单。

```
	<properties>
		<java.version>1.6</java.version>
		<resource.delimiter>@</resource.delimiter> <!-- delimiter that doesn't clash with Spring ${} placeholders -->
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<maven.compiler.source>${java.version}</maven.compiler.source>
		<maven.compiler.target>${java.version}</maven.compiler.target>
	</properties>
```

从上面可以看出，spring boot项目默认的jdk版本是1.6，默认的编码是utf-8。如果我们要修改这些环境参数，比如将jdk版本改成1.8，只需要在我们项目的pom.xml文件中覆盖要修改的参数，如下

```xml
	<properties>
		<java.version>1.8</java.version>
	</properties>
```

## 5、依赖

```xml
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

```

## 6、编译

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
