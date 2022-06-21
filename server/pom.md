
# 一、POM的定义

## pom定义

POM全称是Project Object Model，即项目对象模型。pom.xml是maven的项目描述文件，包含了工程信息和工程的配置细节，Maven使用POM文件来构建工程。

在pom.xml中可以声明的配置包括工程依赖(project dependencies)，插件(plugins)，可执行的目标(goals)，构建配置(build profiles)等等。其他信息，比如工程版本，描述，开发者，邮件列表等等也可以在pom.xml中声明。

pom文件配置的官方地址：https://maven.apache.org/pom.html


## 创建最小化的pom.xml文件

在项目根目录下创建pom.xml文件。

```xml
<project xmlns = "http://maven.apache.org/POM/4.0.0" xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"> 
    <!-- 模型版本 --> 
    <modelVersion>4.0.0</modelVersion> 
    <!-- 公司或者组织的唯一标志，并且配置时生成的路径也是由此生成， 如com.companyname.project-group，maven会将该项目打成的jar包放本地路径：/com/companyname/project-group --> 
    <groupId>com.companyname.project-group</groupId> 
    <!-- 项目的唯一ID，一个groupId下面可能多个项目，就是靠artifactId来区分的 -->
    <artifactId>project-name</artifactId> 
    <!-- 版本号 --> <version>1.0</version>
</project>
``` 
 
所有 POM 文件都需要 project 元素和三个必需字段：groupId，artifactId，version。

project 是工程的根标签

modelVersion要设置为4.0.0

groupId是工程组的标识

artifactId是工程的标识，通常是工程的名称。groupId和artifactId一起定义了artifact在仓库中的位置

version是工程的版本号,在 artifact 的仓库中，它用来区分不同的版本，如：com.companyname.project-group:project-name:1.0

# 二、POM文件结构

Maven项目根目录下的pom.xml文件是Maven项目中非常重要的配置文件。主要描述项目包的依赖和项目构建时的配置。pom.xml配置文件主要分4部分，分别是：

• 项目的描述信息

• 构建时需要的公共变量

• 项目的依赖配置信息

• 构建配置

## 项目描述信息

项目描述信息包括下面的部分：

```xml
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
```

* modelVersion模型版本

```xml
<modelVersion>4.0.0</modelVersion>
```
描述这个POM文件是遵从哪个版本的项目描述符,一般设为：4.0.0

* groupId

针对一个项目的普遍唯一识别符。通常用一个完全正确的包的名字来与其他项目的类似名字来进行区分（比如：org.apache.maven)。

* artifactId 

在给定groupID 的group里面为artifact 指定的标识符是唯一的 ， artifact 代表的是被制作或者被一个project应用的组件(产出物)。

* version

当前项目产生的artifact的版本。

以上4个元素缺一不可，其中groupId, artifactId, version描述依赖的项目唯一标志。


* Parent节点

要成为一个spring boot项目，首先就必须在pom.xml中继承spring-boot-starter-parent，同时指定其版本。

```xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.6.RELEASE</version>
	</parent>
```

* packaging标签

指定项目的打包方式

**<packaging>jar</packaging>**

Jar包是最为常见的打包方式，当pom文件中没有设置packaging参数时，默认使用jar方式打包。

这种打包方式意味着在maven build时会将这个项目中的所有java文件都进行编译形成.class文件，且按照原来的java文件层级结构放置，最终压缩为一个jar文件。

当我们使用mvn install命令的时候，能够发现在项目中与src文件夹同级新生成了一个target文件夹，这个文件夹内的classes文件夹即为刚才提到的编译后形成的文件夹。

  **<packaging>war</packaging>**

war包与jar包非常相似，同样是编译后的.class文件按层级结构形成文件树后打包形成的压缩包。不同的是，它会将项目中依赖的所有jar包都放在WEB-INF/lib这个文件夹下，

WEB-INF/classes文件夹仍然放置我们自己代码的编译后形成的内容。

可想而知，war包非常适合部署时使用，不再需要下载其他的依赖包，能够使用户拿到war包直接使用，因此它经常使用于微服务项目群中的入口项目的pom配置中。

  **<packaging>pom</packaging>**

在父级项目中的pom.xml文件使用的packaging配置一定为pom。父级的pom文件只作项目的子模块的整合，在maven install时不会生成jar/war压缩包。

## 公共变量

在POM.xml中科院定义类似变量，放在properties标签中：

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <java.version>1.8</java.version>
</properties>
```
当需要使用该标签的时候，可以采用：

```
${变量名}
```
例如：下面的标签定义了版本号相关的变量，并应用在依赖标签中。

```xml
<properties>
        <java.version>13</java.version>
        <lombok.version>1.18.10</lombok.version>
    </properties>
      则在Maven的pom.xml中导入lombok依赖的时候，使用如下格式即可定义依赖的版本号：

       <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
            <version>${lombok.version}</version>
        </dependency>
```

## 依赖配置信息

dependencies：配置项目所需要的依赖包，Spring Boot体系内的依赖组件不需要填写具体版本号，spring-boot-starter-parent维护了体系内所有依赖包的版本信息。
dependency：Maven项目定义依赖库的重要标签，通过groupId、artifactId等“坐标”信息定义依赖库的路径信息

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

Maven的一个哲学是惯例优于配置(Convention Over Configuration), Maven默认的依赖配置项中，scope的默认值是compile

* compile
默认就是compile，什么都不配置也就是意味着compile。compile表示被依赖项目需要参与当前项目的编译，当然后续的测试，运行周期也参与其中，是一个比较强的依赖。打包的时候通常需要包含进去。

* test
scope为test表示依赖项目仅仅参与测试相关的工作，包括测试代码的编译，执行。比较典型的如junit。

* runntime
runntime表示被依赖项目无需参与项目的编译，不过后期的测试和运行周期需要其参与。与compile相比，跳过编译而已，说实话在终端的项目（非开源，企业内部系统）中，和compile区别不是很大。比较常见的如JSR×××的实现，对应的API jar是compile的，具体实现是runtime的，compile只需要知道接口就足够了。oracle jdbc驱动架包就是一个很好的例子，一般scope为runntime。另外runntime的依赖通常和optional搭配使用，optional为true。我可以用A实现，也可以用B实现。

* provided 
provided意味着打包的时候可以不用包进去，别的设施(Web Container)会提供。事实上该依赖理论上可以参与编译，测试，运行等周期。相当于compile，但是在打包阶段做了exclude的动作。

* system
从参与度来说，也provided相同，不过被依赖项不会从maven仓库抓，而是从本地文件系统拿，一定需要配合systemPath属性使用。


## 构建配置


Maven是通过pom.xml来执行任务的，其中的build标签描述了如何来编译及打包项目，而具体的编译和打包工作是通过build中配置的 plugin 来完成。当然plugin配置不是必须的，默认情况下，Maven 会绑定以下几个插件来完成基本操作。

| plugin | function | life cycle phase |
| ---- | ---- | ---- |
| maven-clean_plugin|清理上一次执行创建的目标文件|clean|\
| maven-resouces-plugin|处理资源文件和测试文件| resources,testResources|
| maven-compiler-plugin|编译源文件和测试源文件| compile,testCompile|
| maven-surefire-plugin|执行测试文件|test|
| maven-jar-plugin| 创建jar|jar|
| maven-install-plugin| 安装jar，并将创建生成的jar拷贝到.m2/repository下面|install|
| maven-deploy-plugin| 发布jar|deploy|


使用maven构建的项目均可以直接使用maven build完成项目的编译测试打包，无需额外配置。

例如：在没有配置的情况下，执行mvn clean install时，maven会调用默认的plugin来完成编译打包操作，具体来讲，执行mvn clean install时会执行

 maven-clean-plugin:2.5:clean (default-clean)

 maven-resources-plugin:2.6:resources (default-resources)

 maven-compiler-plugin:3.1:compile (default-compile)

 maven-resources-plugin:2.6:testResources (default-testResources)

 maven-compiler-plugin:3.1:testCompile (default-testCompile)

 maven-surefire-plugin:2.12.4:test (default-test)

 maven-jar-plugin:2.4:jar (default-jar)

 maven-install-plugin:2.4:install (default-install)

 等plugin

 


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

## 继承与聚合


