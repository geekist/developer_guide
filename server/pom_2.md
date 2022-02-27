Maven的聚合与继承

## 项目分模块

随着项目扩大，Maven项目也会越来越大。会带来一些麻烦。

1.比如修改了JS文件，但我们依旧要将整个Project都build一下。

2.所有人都在一个Project里面进行更改，可能导致混乱、冲突，也不利于维护。

3.每添加一个jar包或者依赖时，是为整个Project添加的。可见性很大。

因此，为了“高内聚，低耦合”考虑，maven对Project进行模块划分，每个模块都会对应着一个POM.xml文件，然后按照一定的关系组合（继承和聚合）成大的Project。也即是多模块系统。

## 聚合

### 为什么要使用聚合？

***将多个模块组合在一起，统一构建***

把Project模块化成多个项目后，尤其是模块非常多的时候，我们需要一次构建多个项目，而不是到多个模块的目录下分别执行命令进行构建。Maven的聚合特性就是满足该需求的。
把多个模块或项目聚合到一起，我们可以建立一个专门负责聚合工作的project。通过构建这个新的Project，就可以完成整个Project的构建。

### 如何使用聚合？

在聚合工程的pom.xml配置中，增加modules节点

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com</groupId>
	<artifactId>mavenaggregator</artifactId>
 
	<!--  must be pom if aggregator -->
	<packaging>pom</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>my Maven Webapp</name>
	<url>http://maven.apache.org</url>
	
	<modules>
		<!--  relative paths to the directories of pom -->
	    <module>my-project</module>
	    <module>another-project</module>
	</modules>
 
</project>

```

被聚合的模块配置问题：

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com</groupId>
	<artifactId>my-project</artifactId>
	<packaging>jar</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>my-project</name>
	<url>http://maven.apache.org</url>
</project>

```

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com</groupId>
	<artifactId>my-project</artifactId>
	<packaging>jar</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>my-project</name>
	<url>http://maven.apache.org</url>
 
</project>

```

### 使用聚合的注意事项：

* 聚合项目的packaging的标签为pom

* 项目都是在父项目的子目录下，但也可以把子项目放在与父项目同级的地方，只要你修改一下module元素的值即可。

```
<modules>
    <module>../account</module>
    <module>../mail</module>
</modules>
```

## 继承

### 为什么要使用继承：

***解决重复配置的问题***

在Java中为了复用，我们经常使用继承。同样在maven中，也是通过继承来减少重复，增加复用。提取共有部分进行管理，主要解决的是重复配置的问题，通常用于声明一些公共依赖模块、属性等。

### 如何使用继承

* 父类的配置文件设置packaging的标签为：pom

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com</groupId>
	<artifactId>parent-project</artifactId>
	<packaging>pom</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>my-project</name>
	<url>http://maven.apache.org</url>
 
</project>

```

* 子类的配置文件设置父类

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com</groupId>
	<artifactId>child-project</artifactId>
	<packaging>jar</packaging>
	<version>0.0.1-SNAPSHOT</version>
	<name>my-project</name>
	<url>http://maven.apache.org</url>
 
	<parent>
        <groupId>com</groupId>
        <artifactId>parent-project</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </parent>
 
</project>

```

### 使用继承的注意事项

* 父类的packaging位pom类型

* 可以被继承的元素：

| 可以被继承的元素 | 说明 |
| ---- | ---- |
| groupId |	项目组ID，项目坐标的核心坐标； |
version	项目版本，项目坐标的核心坐标；
description	项目的描述信息；
organization	项目的组织信息；
inceptionYear	项目的创始年份；
url	项目的URL地址；
developers	项目的开发者信息；
contributors	项目的贡献值和信息；
distributionManagement	项目的部署配置；
issueManagement	项目的缺陷跟踪系统；
ciManagement	项目的持续集成系统信息；
scm	项目的版本控制系统信息；
mailingLists	项目的邮件列表信息；
properties	自定义的Maven属性；
dependencies	项目的依赖配置；
dependencyManagement	项目的依赖管理配置；
repositories	项目的仓库配置；
build	包括项目的源码目录配置、输出目录配置、插件配置、插件管理配置等；
reporting	包括项目的报告输出目录配置、报告插件配置等。



