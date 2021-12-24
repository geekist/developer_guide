## 2、项目描述文件pom.xml

我们再来看最关键的一个项目描述文件pom.xml，它的内容长得像下面：

```xml
<project ...>
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.itranswarp.learnjava</groupId>
	<artifactId>hello</artifactId>
	<version>1.0</version>
	<packaging>jar</packaging>
	<properties>
        ...
	</properties>
	<dependencies>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
	</dependencies>
</project>
```
### POM介绍

POM全称是Project Object Model，即项目对象模型。pom.xml是maven的项目描述文件，它类似与antx的project.xml文件。pom.xml文件以xml的 形式描述项目的信息，包括项目名称、版本、项目id、项目的依赖关系、编译环境、持续集成、项目团队、贡献管理、生成报表等等。总之，它包含了所有的项目信息。

* modelVersion 

描述这个POM文件是遵从哪个版本的项目描述符。

* groupId

针对一个项目的普遍唯一识别符。通常用一个完全正确的包的名字来与其他项目的类似名字来进行区分（比如：org.apache.maven)。


* artifactId 

在给定groupID 的group里面为artifact 指定的标识符是唯一的 ， artifact 代表的是被制作或者被一个project应用的组件(产出物)。


* version

当前项目产生的artifact的版本。

以上4个元素缺一不可，其中groupId, artifactId, version描述依赖的项目唯一标志。

### POM的基本配置

pom.xml的基本配置。

```xml

<project>  
    <modelVersion>4.0.0</modelVersion>  
    <!- The Basics 项目的基本信息->  
    <groupId>...</groupId>  
    <artifactId>...</artifactId>  
    <version>...</version>  
    <packaging>...</packaging>  
    <dependencies>...</dependencies>  
    <parent>...</parent>  
    <dependencyManagement>...</dependencyManagement>  
    <modules>...</modules>  
    <properties>...</properties>  
    <!- Build Settings 项目的编译设置->  
    <build>...</build>  
    <reporting>...</reporting>  
    <!- More Project Information 其它项目信息 ->  
    <name>...</name>  
    <description>...</description>  
    <url>...</url>  
    <inceptionYear>...</inceptionYear>  
    <licenses>...</licenses>  
    <organization>...</organization>  
    <developers>...</developers>  
    <contributors>...</contributors>  
    <!-- Environment Settings ->  
    <issueManagement>...</issueManagement>  
    <ciManagement>...</ciManagement>  
    <mailingLists>...</mailingLists>   
    <scm>...</scm>  
    <prerequisites>...</prerequisites>  
    <repositories>...</repositories>  
    <pluginRepositories>...</pluginRepositories>  
    <distributionManagement>...</distributionManagement>  
    <profiles>...</profiles>  
</project>  

```

* packaging 标签


maven作为一种XML标记语言，标签通常成对存在，目前packaging标签有3种配置：

```xml
<packaging>pom</packaging>
<packaging>jar</packaging>
<packaging>war</packaging>
```
***<packaging>pom</packaging>***

在父级项目中的pom.xml文件使用的packaging配置一定为pom。父级的pom文件只作项目的子模块的整合，在maven install时不会生成jar/war压缩包。

为什么需要一个父级pom文件呢？

1、可以通过<modules>标签来整合子模块的编译顺序（Maven引入依赖使用最短路径原则，例如a<–b<–c1.0 ，d<–e<–f<–c1.1，由于路径最短，最终引入的为c1.0；但路径长度相同时，则会引入先申明的依赖）。因此尽量将更加底层的service放在更先的位置优先加载依赖较为合适。

2、可以将一些子项目中共用的依赖或将其版本统一写到父级配置中，以便统一管理。

3、groupId, artifactId, version能直接从父级继承，减少子项目的pom配置。

***<packaging>jar</packaging>***

Jar包是最为常见的打包方式，当pom文件中没有设置packaging参数时，默认使用jar方式打包。

这种打包方式意味着在maven build时会将这个项目中的所有java文件都进行编译形成.class文件，且按照原来的java文件层级结构放置，最终压缩为一个jar文件。

当我们使用mvn install命令的时候，能够发现在项目中与src文件夹同级新生成了一个target文件夹，这个文件夹内的classes文件夹即为刚才提到的编译后形成的文件夹。

***<packaging>war</packaging>***

war包与jar包非常相似，同样是编译后的.class文件按层级结构形成文件树后打包形成的压缩包。不同的是，它会将项目中依赖的所有jar包都放在WEB-INF/lib这个文件夹下，

WEB-INF/classes文件夹仍然放置我们自己代码的编译后形成的内容。

可想而知，war包非常适合部署时使用，不再需要下载其他的依赖包，能够使用户拿到war包直接使用，因此它经常使用于微服务项目群中的入口项目的pom配置中。



### POM的依赖关系

```xml
<dependencies>  
    <dependency>  
        <groupId>junit</groupId>  
        <artifactId>junit</artifactId>  
        <version>4.0</version>  
        <type>jar</type>  
        <scope>test</scope>  
        <optional>true</optional>  
    </dependency>  
...  
</dependencies> 
```

Maven会帮下载改依赖的jar包，并放到适当的位置。

默认repository地址：当前用户的私人目录 + .m2

如果你设置了Maven目录下的conf/setting.xml的local repository属性，则不再是默认的repository地址，而使用你指定的地址。

找到下面这段，将它复制一份，放到注释外面，改成你自己的repository路径即可

```
<localRepository>c:\mvn repository\</localRepository>
``` 
当更新了maven的依赖，只需要在idea工程中右键--maven--reimport即可；


