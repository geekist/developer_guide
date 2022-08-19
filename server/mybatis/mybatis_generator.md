- [一、MyBatis Generator介绍](#一mybatis-generator介绍)
  - [1.1 Mybatis Generator介绍](#11-mybatis-generator介绍)
  - [1.2 Mybatis Generator的使用方法](#12-mybatis-generator的使用方法)
- [二、通过Maven插件和xml配置文件运行Mybatis Generator](#二通过maven插件和xml配置文件运行mybatis-generator)
  - [2.1 在项目的pom.xml文件中引入mybatis generator插件](#21-在项目的pomxml文件中引入mybatis-generator插件)
  - [2.2 运行maven命令或者在idea的maven-plugin中直接双击即可](#22-运行maven命令或者在idea的maven-plugin中直接双击即可)
- [三、通过Java代码和xml配置文件运行 Mybatis Generator](#三通过java代码和xml配置文件运行-mybatis-generator)
  - [3.1 在项目的pom.xml文件中引入mybatis generator依赖](#31-在项目的pomxml文件中引入mybatis-generator依赖)
  - [3.2 编写java类读取配置文件](#32-编写java类读取配置文件)
  - [3.3 运行java文件即可生成需要的文件。](#33-运行java文件即可生成需要的文件)
- [四、Mybatis Generator配置文件](#四mybatis-generator配置文件)
  - [4.1 配置文件基本属性](#41-配置文件基本属性)
  - [4.2 properties配置外部配置文件](#42-properties配置外部配置文件)
  - [4.3 context及子元素配置](#43-context及子元素配置)

# 一、MyBatis Generator介绍

## 1.1 Mybatis Generator介绍

MyBatis-Generator是MyBatis 官方提供的一个代码生成工具，可以帮助我们生成数据库表对应的持久化对象（也称作 Model、PO）、操作数据库的接口（dao）、简单 SQL 的 mapper（XML 形式或注解形式）。

MyBatis-Generator （常简写为 MBG 或 mbg）是一个独立工具，你可以下载它的 jar 包来运行，也可以在 Ant 和 Maven 中运行。其官方网址为：mybatis.org/generator/

官网：http://mybatis.org/generator/index.html

Mybatis Generator可以生成的对象：

数据库entity、

接口Mapper

Mapper.xml

## 1.2 Mybatis Generator的使用方法

Mybatis-Generator的运行方式有很多种：

基于mybatis-generator-core-x.x.x.jar和其XML配置文件，通过命令行运行。

通过Ant的Task结合其XML配置文件运行。

通过Maven插件运行。

通过Java代码和其XML配置文件运行。

通过Java代码和编程式配置运行。

通过Eclipse Feature运行。

这里只介绍通过Maven插件运行和通过Java代码和其XML配置文件运行这两种方式，两种方式有个特点：都要提前编写好XML配置文件。个人感觉XML配置文件相对直观，后文会花大量篇幅去说明XML配置文件中的配置项及其作用。这里先注意一点：默认的配置文件为ClassPath:generatorConfig.xml。

# 二、通过Maven插件和xml配置文件运行Mybatis Generator

## 2.1 在项目的pom.xml文件中引入mybatis generator插件

```xml
<build>
<plugins>
    <plugin>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-maven-plugin</artifactId>
        <version>1.4.0</version>
        <executions>
            <execution>
                <id>Generate MyBatis Artifacts</id>
                <goals>
                    <goal>generate</goal>
                </goals>
            </execution>
        </executions>
        <configuration>
            <!-- 输出详细信息 -->
            <verbose>true</verbose>
            <!-- 覆盖生成文件 -->
            <overwrite>true</overwrite>
            <!-- 定义配置文件 -->
            <configurationFile>${basedir}/src/main/resources/generator-configuration.xml</configurationFile>
        </configuration>
    </plugin>
</plugins>
</build>
```

## 2.2 运行maven命令或者在idea的maven-plugin中直接双击即可

**mybatis-generator:generate**

该插件可以通过命令行来运行：

```
mvn mybatis-generator:generate
```

下面的命令演示了一种带参数的命令行方式
```
mvn -Dmybatis.generator.overwrite=true mybatis-generator:generate
```

或者直接在idea编辑器中双击插件即可。


# 三、通过Java代码和xml配置文件运行 Mybatis Generator

## 3.1 在项目的pom.xml文件中引入mybatis generator依赖

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.4.0</version>
</dependency>
```
## 3.2 编写java类读取配置文件

```java
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.Context;
import org.mybatis.generator.config.TableConfiguration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.util.ArrayList;
import java.util.List;

public class GeneratorMapper {
    public static void main(String args[]) throws Exception {
        //1.读取配置文件
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(
                GeneratorMapper.class.getResourceAsStream("/generator/generatorConfig.xml"));
        Context context=config.getContext("Mysql");

        //2.生成generator类实例
        List<TableConfiguration> configs= config.getContext("Mysql").getTableConfigurations();
        for(TableConfiguration c:configs){
            c.setCountByExampleStatementEnabled(false);
            c.setUpdateByExampleStatementEnabled(false);
            c.setUpdateByPrimaryKeyStatementEnabled(false);
            c.setDeleteByExampleStatementEnabled(false);
            c.setSelectByExampleStatementEnabled(false);
        }
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);

        //3.生成实体类entity、mapper和mapper.xml文件
        myBatisGenerator.generate(null);
    }
}
```

## 3.3 运行java文件即可生成需要的文件。


# 四、Mybatis Generator配置文件


## 4.1 配置文件基本属性

一个典型的配置文件如下：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--mybatis的代码生成器相关配置-->
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="generator.properties"/>

    <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
        property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
        javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
    -->
    <context id="myContext" targetRuntime="MyBatis3Simple" defaultModelType="flat">
        <!--        设置生成文件的编码-->
        <property name="javaFileEncoding" value="UTF-8"/>

        <!-- 这个插件给生成的Java模型对象增加了equals和hashCode方法 -->
        <plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>
        <!--        增加 toString 方法-->
        <plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>
        <!--        生成序列化方法-->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>

        <!-- 注释 -->
        <commentGenerator>
            <!-- 是否生成注释 true： 否： false: 是 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 是否去除时间戳 true：是 ： false:否 -->
            <property name="suppressDate" value="true"/>
            <!-- 是否添加数据库表中字段的注释 true：是 ： false:否，只有当suppressAllComments 为 false 时才能生效 -->
            <property name="addRemarkComments" value="true"/>
        </commentGenerator>


        <!-- jdbc连接 -->
        <jdbcConnection driverClass="${jdbc.driver-class-name}"
                        connectionURL="${jdbc.url}"
                        userId="${jdbc.username}"
                        password="${jdbc.password}">
            <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!--是否使用bigDecimal，默认false。
                false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
                true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
            <property name="forceBigDecimals" value="true"/>
            <!--默认 false
                false，将所有 JDBC 的时间类型解析为 java.util.Date
                true，将 JDBC 的时间类型按如下规则解析
                    DATE	                -> java.time.LocalDate
                    TIME	                -> java.time.LocalTime
                    TIMESTAMP               -> java.time.LocalDateTime
                    TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
                    TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
                -->
            <property name="useJSR310Types" value="false"/>
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="com.cunyu1943.mybatisgeneratordemo.entity" targetProject="src/main/java">
            <!-- 是否让 schema 作为包的后缀，默认为false -->
            <property name="enableSubPackages" value="false"/>
            <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成Mapper.xml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
        </sqlMapGenerator>

        <!-- 生成 XxxMapper.java 接口-->
        <javaClientGenerator targetPackage="com.cunyu1943.mybatisgeneratordemo.mapper" targetProject="src/main/java"
                             type="XMLMAPPER">
        </javaClientGenerator>


        <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
             tableName为对应的数据库表名
             domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
             enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
             -->
        <table schema="" tableName="user" domainObjectName="User"
               enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false" selectByExampleQueryId="false">
            <!--是否使用实际列名,默认为false-->
            <property name="useActualColumnNames" value="false"/>
        </table>
    </context>
</generatorConfiguration>
```

配置文件中，最外层的标签为<generatorConfiguration>，它的子标签包括：

**properties标签**
0或者1个<properties>标签，用于指定全局配置文件，下面可以通过占位符的形式读取<properties>指定文件中的值。

**classPathEntry标签**
0或者N个<classPathEntry>标签，<classPathEntry>只有一个location属性，用于指定数据源驱动包（jar或者zip）的绝对路径，具体选择什么驱动包取决于连接什么类型的数据源。

**context标签**
1或者N个<context>标签，用于运行时的解析模式和具体的代码生成行为，所以这个标签里面的配置是最重要的。


下面分别列举和分析一下<context>标签和它的主要子标签的一些属性配置和功能。


## 4.2 properties配置外部配置文件

我们一般引入外部文件，主要用于配置项目数据库，方便我们后续的设置，引入外部配置文件的方式也很简单，具体配置如下：

```xml
<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="generator.properties"/>
</generatorConfiguration>
```

数据库配置文件generator.properties如下：注意properties文件目录一遍在resources目录下，否则不会被识别。

```
#配置数据库选项，也可以直接在xml文件中配置
jdbc.username=employ
jdbc.password=employ_2020
jdbc.driver-class-name=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/qlm_biz
```

## 4.3 context及子元素配置

context元素配置

```xml
<context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat"></context>
```

其中的各个属性含义如下：

* id：唯一标识，不可重复，可以根据我们自己的喜好进行自定义。
  
* defaultModelType：非必填项，有两个值可选，一个是 conditional，也是默认值，另一个值是 flat，也就是我们常用的一个配置，表示数据库中的一张表对应生成一个 PO。
  
* targetRuntime：非必填项，这里同样有两个值可选，一个是 MyBatis3，一个是 MyBatis3Simple，两者的最主要区别在于不同配置下所生成的 DAO 和 Mapper 会有所不同，后者生成的 DAO 和 Mapper 会少很多，只含有日常最常用的。

context 子元素配置

context 除了上面配置的之外，还有许多子元素需要配置，而且这些子元素的配置的个数以及顺序都是规定好的，如果不按照给定的规则进行配置，则会导致错误，常见子元素及个数配置如下（按照规定的顺序进行从上到下排序）：

| 子元素 | 最少个数 | 最多个数 |
| ---- | ---- | ---- |
| property | 0 | N |
|plugin	| 0 |	N |
|commentGenerator|	0|	1|
|jdbcConnection| | |		
|javaTypeResolver|	0|	1|
|javaModelGenerator|	1|	N|
|sqlMapGenerator|	0|	1|
|javaClientGenerator|	0|	1|
|table	|1	|N|


* property

用于为代码生成指定属性，或为其它元素指定属性。可以配置零个或多个，常见的 property 配置如下：
```xml
<!-- 自动识别数据库关键字，默认为 false，一般保留默认值，遇到数据库关键字（Java关键字）时，按照 table 元素中 columnOverride 属性的配置进行覆盖；
  如果设置为 true， 则需按照 SqlReservedWords 中定义的关键字列表，对关键字进行定界（分隔）；
  定界符（分隔符）参见 beginningDelimiter 和 endingDelimiter 的设置-->
<property name="autoDelimitKeywords" value="false"/>

<!-- beginningDelimiter 和 endingDelimiter，定界符（分隔符），指明用于标记数据库关键字的符号，默认为为双引号 (")；
  在 oracle 中是双引号 (")，在 MySQL 中需配置为反引号 (`)  -->
<property name="beginningDelimiter" value="`"/>
<property name="endingDelimiter" value="`"/>

<!-- 生成的 Java 文件的编码   -->
<property name="JavaFileEncoding" value="UTF-8"/>

<!-- 格式化 Java 代码 -->
<property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
<!-- 格式化 XML 代码 -->
<property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>
```

如果我们要给我们的所生成文件的编码类型进行设置，则可以在此处进行配置，具体配置如下：
```xml
<property name="javaFileEncoding" value="UTF-8"/>
```

* plugin

默认生成的 PO 中，只包含了各个各个属性声明以及各个属性所对应的 setter/getter，如果我们需要对生成的实体类文件增加一些属性，可以通过插件来实现。


```xml
<!-- 使生成的 Model 实现 Serializable 接口  -->
<plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>

<!--  为生成的 Model 覆写 toString() 方法 -->
<plugin type="org.mybatis.generator.plugins.ToStringPlugin"/>

<!--  为生成的 Model 覆写 equals() 和 hashCode() 方法 -->
<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>
```

为模型生成序列化方法，则使用如下插件：
```
<plugin type="org.mybatis.generator.plugins.SerializablePlugin"/>
```

* commentGenerator

该配置主要用于配置生成的注释，默认情况下是会生成注释的，而且会带上时间戳，如果我们不需要这些配置，则可以通过如下配置来清除：
```xml
<commentGenerator>
    <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
    <property name="suppressAllComments" value="true"/>
    <!-- 是否去除自动生成的时间戳 true：是 ： false:否 -->
    <property name="suppressDate" value="true"/>
    <!-- 是否添加数据库表中字段的注释 true：是 ： false:否，只有当suppressAllComments 为 false 时才能生效 -->   
    <property name="addRemarkComments" value="true"/>
</commentGenerator>
```

* jdbcConnection

既然要自动生成对应文件，那肯定得链接数据库，所以我们需要对数据库进行配置，上面我们讲过导入外部配置文件，我们可以通过这种方式将数据库的配置定义在外部文件中，然后通过导入该文件进行配置即可，具体可以通过如下具体步骤进行：
```xml
<jdbcConnection driverClass="${jdbc.driver-class-name}"
                connectionURL="${jdbc.url}"
                userId="${jdbc.username}"
                password="${jdbc.password}">
    <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
    <property name="nullCatalogMeansCurrent" value="true"/>
</jdbcConnection>
```

* javaTypeResolver

主要用于配置 JDBC 和 Java 中的类型转换规则，如果我们不配置，会采用默认的一套转换规则，而如果我们需要自定义，也只能配置 bigDecimal、NUMERIC 和时间类型，不能去配置其他类型，否则会导致出错，具体配置规则如下：
```xml
<javaTypeResolver>
    <property name="forceBigDecimals" value="true"/>
    <property name="useJSR310Types" value="false"/>
</javaTypeResolver>
```

* forceBigDecimals

该属性默认为 false，此时它会将 JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，若该属性为 true，此时将会把 JDBC DECIMAL 和 NUMERIC 类型解析为 java.math.BigDecimal。

* useJSR310Types

该属性默认为 false，它会将 JDBC 所有的时间类型都解析为 java.util.Date，若该属性为 true，则会按照如下规则进行解析：

| 转换前 | 转换后 |
| ---- | ---- |
| DATE	| java.time.LocalDate |
| TIME	| java.time.LocalTime |
| TIMESTAMP	| java.time.LocalDateTime |
| TIME_WITH_TIMEZONE | java.time.OffsetTime |
| TIMESTAMP_WITH_TIMEZONE | java.time.OffsetDateTime |

* javaModelGenerator

这里主要用于配置自动生成的 PO 所在的包路径和项目路径，这里需要根据自己的需求进行配置，这里以我自己的配置为例，比如我的 PO 所在包为 com.cunyu1943.mybatisgeneratordemo.entity，项目路径为 src/main/java。

```xml
<javaModelGenerator targetPackage="com.cunyu1943.mybatisgeneratordemo.entity" targetProject="src/main/java">
    <!-- 是否让 schema 作为包的后缀，默认为 false -->
    <property name="enableSubPackages" value="false"/>
    <!-- 是否针对 String 类型的字段在 set 方法中进行修剪，默认 false -->
    <property name="trimStrings" value="true"/>
</javaModelGenerator>
```

* sqlMapGenerator

配置生成的 Mapper.xml 所存放的路径，比如我们要放在 src/main/resources/mapper 路径下，则配置如下：

```xml
<sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
</sqlMapGenerator>
```
* javaClientGenerator

配置 Mapper 接口所存放的路径，一般我们都是存放在项目的 mapper 包下，如我的配置为：

```xml
<javaClientGenerator targetPackage="com.cunyu1943.mybatisgeneratordemo.mapper" targetProject="src/main/java"
                     type="XMLMAPPER">
</javaClientGenerator>
```

* table

配置所要自动生成代码的数据库表，这里一张表对应一个 table，如果要生成多张表，则需要配置多个 table，以下为一个具体实例：

```xml
<table schema="" tableName="user" domainObjectName="User"
       enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
       enableUpdateByExample="false" selectByExampleQueryId="false">
    <!--是否使用实际列名,默认为false-->
    <property name="useActualColumnNames" value="false" />
</table>
```

其中，schema 是数据库名，有的数据库需要配置，有的数据库不需要配置，这里需要具体根据你自己所用的数据库来填写，不过建议都填上，方便不同数据库也可以适用。tableName 则对应数据库表名；domainObjectName 对应生成的实体类名，默认可以不用配置，不配置时它将按照帕斯卡命名法将表明转换为类名；而 enableXXXByExample 默认为 true，默认会生成一个 Example 帮助类，不过该配置只有在 targetRuntime="MyBatis3" 时才能生效，当 targetRuntime="MyBatis3Simple" 时，enableXXXByExample 无论如何配置都不起作用。

