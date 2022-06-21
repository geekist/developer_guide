
# 一、POM的定义

## 1.pom定义

POM全称是Project Object Model，即项目对象模型。pom.xml是maven的项目描述文件，包含了工程信息和工程的配置细节，Maven使用POM文件来构建工程。

在pom.xml中可以声明的配置包括工程依赖(project dependencies)，插件(plugins)，可执行的目标(goals)，构建配置(build profiles)等等。其他信息，比如工程版本，描述，开发者，邮件列表等等也可以在pom.xml中声明。

pom文件配置的官方地址：https://maven.apache.org/pom.html


## 2.创建最小化的pom.xml文件

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

## 1.项目描述信息

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

## 2.公共变量

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

## 3.依赖配置信息

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

dependencyManagement

使用dependencyManagement可以统一管理项目的版本号，确保应用的各个项目的依赖和版本一致，不用每个模块项目都弄一个版本号，不利于管理，当需要变更版本号的时候只需要在父类容器里更新，不需要任何一个子项目的修改；如果某个子项目需要另外一个特殊的版本号时，只需要在自己的模块dependencies中声明一个版本号即可。子类就会使用子类声明的版本号，不继承于父类版本号。

与dependencies区别：

1)Dependencies相对于dependencyManagement，所有生命在dependencies里的依赖都会自动引入，并默认被所有的子项目继承。

2)dependencyManagement里只是声明依赖，并不自动实现引入，因此子项目需要显示的声明需要用的依赖。如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom;另外如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

## 4.构建配置

### 默认build插件

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


### build插件配置的两种方式

在Maven的pom.xml文件中，存在如下两种<build>：

（1）全局配置（project build）

针对整个项目的所有情况都有效

（2）配置（profile build）

针对不同的profile配置

```xml
<project 
  <!-- "Project Build" contains elements of the BaseBuild set and the Build set-->  
  <build>...</build>  
   
  <profiles>  
    <profile>  
      <!-- "Profile Build" contains elements of the BaseBuild set only -->  
      <build>...</build>  
    </profile>  
  </profiles>  
</project>
```
###  配置说明

基本元素

* defaultGoal
 
 执行build任务时，如果没有指定目标，将使用的默认值。如下配置：在命令行中执行mvn，则相当于执行mvn install

* directory
 
build目标文件的存放目录，默认在${basedir}/target目录

* finalName

build目标文件的名称，默认情况为${artifactId}-${version}

* filter

定义*.properties文件，包含一个properties列表，该列表会应用到支持filter的resources中。

也就是说，定义在filter的文件中的name=value键值对，会在build时代替${name}值应用到resources中。

maven的默认filter文件夹为${basedir}/src/main/filters

```xml
<build>  
  <defaultGoal>install</defaultGoal>  
  <directory>${basedir}/target</directory>  
  <finalName>${artifactId}-${version}</finalName> 
  <filters>
   <filter>filters/filter1.properties</filter>
  </filters> 
  ...
</build> 
```          

Resources配置

用于包含或者排除某些资源文件

* resources

一个resources元素的列表。每一个都描述与项目关联的文件是什么和在哪里

* targetPath

指定build后的resource存放的文件夹，默认是basedir。

通常被打包在jar中的resources的目标路径是META-INF

* filtering

true/false，表示为这个resource，filter是否激活

* directory

定义resource文件所在的文件夹，默认为${basedir}/src/main/resources

* includes

指定哪些文件将被匹配，以*作为通配符

* excludes

指定哪些文件将被忽略

* testResources

定义和resource类似，只不过在test时使用

```xml
<build>  
        ...  
       <resources>  
          <resource>  
             <targetPath>META-INF/plexus</targetPath>  
             <filtering>true</filtering>  
            <directory>${basedir}/src/main/plexus</directory>  
            <includes>  
                <include>configuration.xml</include>  
            </includes>  
            <excludes>  
                <exclude>**/*.properties</exclude>  
            </excludes>  
         </resource>  
    </resources>  
    <testResources>  
        ...  
    </testResources>  
    ...  
</build>  
```   

plugins配置

用于指定使用的插件

pluginManagement配置

pluginManagement的配置和plugins的配置是一样的，只是用于继承，使得可以在孩子pom中使用。

```xml
<build>  
    ...  
    <plugins>  
        <plugin>  
            <groupId>org.apache.maven.plugins</groupId>  
            <artifactId>maven-jar-plugin</artifactId>  
            <version>2.0</version>  
            <extensions>false</extensions>  
            <inherited>true</inherited>  
            <configuration>  
                <classifier>test</classifier>  
            </configuration>  
            <dependencies>...</dependencies>  
            <executions>...</executions>  
        </plugin>  
    </plugins>  
</build> 
``` 

在<build>中，<pluginManagement>与<plugins>并列，两者之间的关系类似于<dependencyManagement>与<dependencies>之间的关系。<pluginManagement>中也配置<plugin>，其配置参数与<plugins>中的<plugin>完全一致。只是，<pluginManagement>往往出现在父项目中，其中配置的<plugin>往往通用于子项目。子项目中只要在<plugins>中以<plugin>声明该插件，该插件的具体配置参数则继承自父项目中<pluginManagement>对该插件的配置，从而避免在子项目中进行重复配置。

## 5.继承与聚合


### 项目分模块

随着项目扩大，Maven项目也会越来越大。会带来一些麻烦。

1.比如修改了JS文件，但我们依旧要将整个Project都build一下。

2.所有人都在一个Project里面进行更改，可能导致混乱、冲突，也不利于维护。

3.每添加一个jar包或者依赖时，是为整个Project添加的。可见性很大。

因此，为了“高内聚，低耦合”考虑，maven对Project进行模块划分，每个模块都会对应着一个POM.xml文件，然后按照一定的关系组合（继承和聚合）成大的Project。也即是多模块系统。

### 聚合

#### 为什么要使用聚合？

***将多个模块组合在一起，统一构建***

把Project模块化成多个项目后，尤其是模块非常多的时候，我们需要一次构建多个项目，而不是到多个模块的目录下分别执行命令进行构建。Maven的聚合特性就是满足该需求的。
把多个模块或项目聚合到一起，我们可以建立一个专门负责聚合工作的project。通过构建这个新的Project，就可以完成整个Project的构建。

#### 如何使用聚合？

在聚合工程的pom.xml配置中，增加modules节点

```xml
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

```xml
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

```xml
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

#### 使用聚合的注意事项：

* 聚合项目的packaging的标签为pom

* 项目都是在父项目的子目录下，但也可以把子项目放在与父项目同级的地方，只要你修改一下module元素的值即可。

```
<modules>
    <module>../account</module>
    <module>../mail</module>
</modules>
```

### 继承

#### 为什么要使用继承：

***解决重复配置的问题***

在Java中为了复用，我们经常使用继承。同样在maven中，也是通过继承来减少重复，增加复用。提取共有部分进行管理，主要解决的是重复配置的问题，通常用于声明一些公共依赖模块、属性等。

#### 如何使用继承

* 父类的配置文件设置packaging的标签为：pom

```xml
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

```xml
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

#### 使用继承的注意事项

* 父类的packaging位pom类型

* 可以被继承的元素：

| 可以被继承的元素 | 说明 |
| ---- | ---- |
| groupId |	项目组ID，项目坐标的核心坐标； |
| version  |	项目版本，项目坐标的核心坐标； |
| description	|项目的描述信息；|
| organization	|项目的组织信息；|
| inceptionYear	|项目的创始年份；|
| url	| 项目的URL地址；|
| developers	| 项目的开发者信息；|
| contributors	| 项目的贡献值和信息；|
| distributionManagement	|项目的部署配置；|
| issueManagement	|项目的缺陷跟踪系统；|
| ciManagement	|项目的持续集成系统信息；|
| scm	|项目的版本控制系统信息；|
| mailingLists	|项目的邮件列表信息；|
| properties	|自定义的Maven属性；|
| dependencies	|项目的依赖配置；|
| dependencyManagement	|项目的依赖管理配置；|
| repositories	|项目的仓库配置；|
| build	|包括项目的源码目录配置、输出目录配置、插件配置、插件管理配置等；|
| reporting	|包括项目的报告输出目录配置、报告插件配置等。|

### 聚合与继承在一个项目中同时使用

一个项目往往既使用了聚合，又使用了继承。


