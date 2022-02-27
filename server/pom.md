

## pom定义

POM全称是Project Object Model，即项目对象模型。pom.xml是maven的项目描述文件，它类似与antx的project.xml文件。pom.xml文件以xml的 形式描述项目的信息，包括项目名称、版本、项目id、项目的依赖关系、编译环境、持续集成、项目团队、贡献管理、生成报表等等。总之，它包含了所有的项目信息。


***一个最简单的pom.xml文件***

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

描述这个POM文件是遵从哪个版本的项目描述符。

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

* groupId

针对一个项目的普遍唯一识别符。通常用一个完全正确的包的名字来与其他项目的类似名字来进行区分（比如：org.apache.maven)。

* artifactId 

在给定groupID 的group里面为artifact 指定的标识符是唯一的 ， artifact 代表的是被制作或者被一个project应用的组件(产出物)。

* version

当前项目产生的artifact的版本。

以上4个元素缺一不可，其中groupId, artifactId, version描述依赖的项目唯一标志。


## 3、Parent节点

要成为一个spring boot项目，首先就必须在pom.xml中继承spring-boot-starter-parent，同时指定其版本。

```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
	</parent>
```

## 打包方式

* packaging 标签


maven作为一种XML标记语言，标签通常成对存在，目前packaging标签有3种配置：

```xml
<packaging>pom</packaging>
<packaging>jar</packaging>
<packaging>war</packaging>
```

* <packaging>jar</packaging>

Jar包是最为常见的打包方式，当pom文件中没有设置packaging参数时，默认使用jar方式打包。

这种打包方式意味着在maven build时会将这个项目中的所有java文件都进行编译形成.class文件，且按照原来的java文件层级结构放置，最终压缩为一个jar文件。

当我们使用mvn install命令的时候，能够发现在项目中与src文件夹同级新生成了一个target文件夹，这个文件夹内的classes文件夹即为刚才提到的编译后形成的文件夹。

* <packaging>war</packaging>

war包与jar包非常相似，同样是编译后的.class文件按层级结构形成文件树后打包形成的压缩包。不同的是，它会将项目中依赖的所有jar包都放在WEB-INF/lib这个文件夹下，

WEB-INF/classes文件夹仍然放置我们自己代码的编译后形成的内容。

可想而知，war包非常适合部署时使用，不再需要下载其他的依赖包，能够使用户拿到war包直接使用，因此它经常使用于微服务项目群中的入口项目的pom配置中。

* <packaging>pom</packaging> 

在父级项目中的pom.xml文件使用的packaging配置一定为pom。父级的pom文件只作项目的子模块的整合，在maven install时不会生成jar/war压缩包。

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
