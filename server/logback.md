
# 一、Logback介绍

## 1、logback介绍

Logback是由log4j创始人设计的又一个开源日志组件。logback当前分成三个模块：logback-core,logback- classic和logback-access。logback-core是其它两个模块的基础模块。logback-classic是log4j的一个 改良版本。此外logback-classic完整实现SLF4J API使你可以很方便地更换成其它日志系统如log4j或JDK14 Logging。

logback的官方网站： http://logback.qos.ch

SLF4J的官方网站：http://www.slf4j.org

## 2、logback特点

 1、更快的实现  Logback的内核重写了，在一些关键执行路径上性能提升10倍以上。而且logback不仅性能提升了，初始化内存加载也更小了。

2、非常充分的测试  Logback经过了几年，数不清小时的测试。Logback的测试完全不同级别的。在作者的观点，这是简单重要的原因选择logback而不是log4j。

3、Logback-classic非常自然实现了SLF4j    Logback-classic实现了 SLF4j。在使用SLF4j中，你都感觉不到logback-classic。而且因为logback-classic非常自然地实现了SLF4J，  所 以切换到log4j或者其他，非常容易，只需要提供成另一个jar包就OK，根本不需要去动那些通过SLF4JAPI实现的代码。

4、非常充分的文档  官方网站有两百多页的文档。

5、自动重新加载配置文件  当配置文件修改了，Logback-classic能自动重新加载配置文件。扫描过程快且安全，它并不需要另外创建一个扫描线程。这个技术充分保证了应用程序能跑得很欢在JEE环境里面。

6、Lilith   Lilith是log事件的观察者，和log4j的chainsaw类似。而lilith还能处理大数量的log数据 。

7、谨慎的模式和非常友好的恢复  在谨慎模式下，多个FileAppender实例跑在多个JVM下，能 够安全地写道同一个日志文件。RollingFileAppender会有些限制。Logback的FileAppender和它的子类包括 RollingFileAppender能够非常友好地从I/O异常中恢复。

8、配置文件可以处理不同的情况   开发人员经常需要判断不同的Logback配置文件在不同的环境下（开发，测试，生产）。而这些配置文件仅仅只有一些很小的不同，可以通过,和来实现，这样一个配置文件就可以适应多个环境。

9、Filters（过滤器）  有些时候，需要诊断一个问题，需要打出日志。在log4j，只有降低日志级别，不过这样会打出大量的日志，会影响应用性能。在Logback，你可以继续 保持那个日志级别而除掉某种特殊情况，如alice这个用户登录，她的日志将打在DEBUG级别而其他用户可以继续打在WARN级别。要实现这个功能只需 加4行XML配置。可以参考MDCFIlter 。

10、SiftingAppender（一个非常多功能的Appender）  它可以用来分割日志文件根据任何一个给定的运行参数。如，SiftingAppender能够区别日志事件跟进用户的Session，然后每个用户会有一个日志文件。

11、自动压缩已经打出来的log  RollingFileAppender在产生新文件的时候，会自动压缩已经打出来的日志文件。压缩是个异步过程，所以甚至对于大的日志文件，在压缩过程中应用不会受任何影响。

12、堆栈树带有包版本  Logback在打出堆栈树日志时，会带上包的数据。

13、自动去除旧的日志文件  通过设置TimeBasedRollingPolicy或者SizeAndTimeBasedFNATP的maxHistory属性，你可以控制已经产生日志文件的最大数量。如果设置maxHistory 12，那那些log文件超过12个月的都会被自动移除。

# 二、Logback配置文件

Logback最简单的使用就是不使用任何配置文件，用默认的Console输出日志，如果配置了Logback，则使用logback.xml配置文件

## 1、Logback的配置文件

Logback 配置文件的语法非常灵活。正因为灵活，所以无法用 DTD 或 XML schema 进行定义。尽管如此，可以这样描述配置文件的基本结构：以<configuration>开头，后面有零个或多个<appender>元素，有零个或多个<logger>元素，有最多一个<root>元素。

Logback默认配置的步骤

(1). 尝试在 classpath 下查找文件 logback-test.xml；

(2). 如果文件不存在，则查找文件 logback.xml；

(3). 如果两个文件都不存在，logback 用 Bas icConfigurator 自动对自己进行配置，这会导致记录输出到控制台。

## 2、Logback配置文件组成

1) configuration是根标签，有三个属性debug，scan和scanPeriod，debug=true时会打印出logback内部的状态，scan=true表示如果配置修改了，会重新加载模式为true，scanPeriod为扫描时间间隔。

2) logger标签用来设置某一个包或具体的某一个类的日志输出，有个level属性，用来设置日志级别，可选值为TRACE，DEBUG，INFO，WARN， ERROR， ALL 和 OFF。logger标签可以包括零个或者多个appender-ref标签。

3) root标签配置root logger，这个标签和logger一样有个level，也有零个或者多个appender-ref标签。

4) Appender标签，负责写日志的组件，有两个必要属性name和class，name指定appender名称，class指定appender的全限定名。这个Appender可以有零个或者多个layout，encoder和filter标签。appender的class 类型主要有三种：ConsoleAppender、FileAppender、RollingFileAppender。


```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    Copyright 2010-2011 The myBatis Team
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
        http://www.apache.org/licenses/LICENSE-2.0
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
-->
<configuration debug="false">
    <!--定义日志文件的存储地址 勿在 LogBack 的配置中使用相对路径-->  
    <property name="LOG_HOME" value="/home" />  
    <!-- 控制台输出 -->   
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder"> 
             <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符--> 
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>   
        </encoder> 
    </appender>
    <!-- 按照每天生成日志文件 -->   
    <appender name="FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">   
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--日志文件输出的文件名-->
            <FileNamePattern>${LOG_HOME}/TestWeb.log.%d{yyyy-MM-dd}.log</FileNamePattern> 
            <!--日志文件保留天数-->
            <MaxHistory>30</MaxHistory>
        </rollingPolicy>   
        <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder"> 
            <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符--> 
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>   
        </encoder> 
        <!--日志文件最大的大小-->
       <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
         <MaxFileSize>10MB</MaxFileSize>
       </triggeringPolicy>
    </appender> 
   <!-- show parameters for hibernate sql 专为 Hibernate 定制 --> 
    <logger name="org.hibernate.type.descriptor.sql.BasicBinder"  level="TRACE" />  
    <logger name="org.hibernate.type.descriptor.sql.BasicExtractor"  level="DEBUG" />  
    <logger name="org.hibernate.SQL" level="DEBUG" />  
    <logger name="org.hibernate.engine.QueryParameters" level="DEBUG" />
    <logger name="org.hibernate.engine.query.HQLQueryPlan" level="DEBUG" />  
    
    <!--myibatis log configure--> 
    <logger name="com.apache.ibatis" level="TRACE"/>
    <logger name="java.sql.Connection" level="DEBUG"/>
    <logger name="java.sql.Statement" level="DEBUG"/>
    <logger name="java.sql.PreparedStatement" level="DEBUG"/>
    
    <!-- 日志输出级别 -->
    <root level="INFO">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </root> 
     <!--日志异步到数据库 -->  
    <appender name="DB" class="ch.qos.logback.classic.db.DBAppender">
        <!--日志异步到数据库 --> 
        <connectionSource class="ch.qos.logback.core.db.DriverManagerConnectionSource">
           <!--连接池 --> 
           <dataSource class="com.mchange.v2.c3p0.ComboPooledDataSource">
              <driverClass>com.mysql.jdbc.Driver</driverClass>
              <url>jdbc:mysql://127.0.0.1:3306/databaseName</url>
              <user>root</user>
              <password>root</password>
            </dataSource>
        </connectionSource>
  </appender>
</configuration>

```

# 三、使用Logback

```java

package com.stu.system.action; 

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class BlogAction{
     //定义一个全局的记录器，通过LoggerFactory获取
     private final static Logger logger = LoggerFactory.getLogger(BlogAction.class); 
     /**
     * @param args
     */
    public static void main(String[] args) {
        logger.info("logback 成功了");
        logger.error("logback 成功了");
    }
}
```

# 四、Spring boot中使用Logback

## spring boot 默认使用Logback

Spring boot默认使用Logback作为内置的日志系统，假设你使用starter启动创建SpringBoot应用，则默认已经导入了spring-boot-starter-logging的依赖，相继也就导入了logback所需要的依赖。

引入spring-boot-starter，会自动引入spring-boot-starter-logging

引入spring-boot-starter-web，会自动引入spring-boot-starter

也可以直接使用logback的依赖
```xml
 <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.1.11.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```

## 在spring.ymal中配置logback的设置：

指定logback的配置文件名称和文件输出路径

```
logging:
  config: classpath:logback-spring.xml

logback:
  logPath: /home
  subLogPath: /logs
```

指定logback的配置级别

```
logging:
  level:
    com.example.logbackdemo: debug # 指定包下日志以debug级别输出
    #root: warn # root日志以warn级别输出，不要用debug级别！！---会卡的！
```
