# 一、TLog介绍

## 简介：

TLog提供了一种最简单的方式来解决日志追踪问题，它不收集日志，也不需要另外的存储空间，它只是自动的对你的日志进行打标签，自动生成TraceId贯穿你微服务的一整条链路。并且提供上下游节点信息。适合中小型企业以及想快速解决日志追踪问题的公司项目使用。

## 官网地址：

https://tlog.yomahub.com/


## Tlog特点：

通过对日志打标签完成轻量级微服务日志追踪

提供三种接入方式：javaagent完全无侵入接入，字节码一行代码接入，基于配置文件的接入

对业务代码无侵入式设计，使用简单，10分钟即可接入

支持常见的log4j，log4j2，logback三大日志框架，并提供自动检测，完成适配

支持dubbo，dubbox，springcloud三大RPC框架

支持Spring Cloud Gateway和Soul网关

适配HttpClient和Okhttp的http调用标签传递

支持三种任务框架，JDK的TimerTask，Quartz，XXL-JOB

支持日志标签的自定义模板的配置，提供多个系统级埋点标签的选择

支持异步线程的追踪，包括线程池，多级异步线程等场景

几乎无性能损耗，快速稳定，经过压测，损耗在0.01%

tlog的

# 二、TLog使用方法：

## 安装依赖

```xml
<dependency>
  <groupId>com.yomahub</groupId>
  <artifactId>tlog-all-spring-boot-starter</artifactId>
  <version>1.4.3</version>
</dependency>
```

## 使用方式一：javaagent

使用javaagent方式，只需要在你的java启动参数中加入：

```
-javaagent:/your_path/tlog-agent.jar
```

最新的tlog-agent.jar可以在以下地址进行下载：https://gitee.com/dromara/TLog/releases/v1.4.3

## 使用方式二：字节码注入

只需要在你的启动类中加入一行代码，即可以自动进行探测你项目所使用的Log框架，并进行增强。

```
@SpringBootApplication
public class Runner {

    static {AspectLogEnhance.enhance();}//进行日志增强，自动判断日志框架

    public static void main(String[] args) {
        SpringApplication.run(Runner.class, args);
    }
}
```

## 使用方式三：配置日志框架

换掉encoder的实现类或者换掉layout的实现类就可以了


```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="false">
    <property name="APP_NAME" value="logtest"/>
    <property name="LOG_HOME" value="./logs" />
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <!--这里替换成AspectLogbackEncoder-->
        <encoder class="com.yomahub.tlog.core.enhance.logback.AspectLogbackEncoder">
              <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>
    <appender name="FILE"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <File>${LOG_HOME}/${APP_NAME}.log</File>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <FileNamePattern>${LOG_HOME}/${APP_NAME}.log.%d{yyyy-MM-dd}.%i.log</FileNamePattern>
            <MaxHistory>30</MaxHistory>
            <maxFileSize>1000MB</maxFileSize>
        </rollingPolicy>
        <!--这里替换成AspectLogbackEncoder-->
        <encoder class="com.yomahub.tlog.core.enhance.logback.AspectLogbackEncoder">
            <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="STDOUT" />
        <appender-ref ref="FILE" />
    </root>
</configuration>

```


不使用tlog的日志格式：

```
2022-07-10 10:30:25.612  INFO 12568 --- [erListUpdater-0] c.netflix.config.ChainedDynamicProperty  115 : Flipping property: yychildren-core.ribbon.ActiveConnectionsLimit to use NEXT property: niws.loadbalancer.availabilityFilteringRule.activeConnectionsLimit = 2147483647
2022-07-10 10:30:26.211  INFO 12568 --- [io-50004-exec-2] com.yuya.common.utils.JwtTokenUtils      121 : jwt过期时间：Mon Jul 10 10:30:26 CST 2023
2022-07-10 10:30:26.278  INFO 12568 --- [io-50004-exec-2] c.y.t.controller.UTeacherController      96 : [UTeacherController]======= 登录成功 =======，Token=Hzyy10 eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxNzMwNjgxODk3MiIsImF1ZCI6InJlc3RhcGl1c2VyIiwibmJmIjoxNjU3NDIwMjI2LCJpc3MiOiIwOThmNmJjZDQ2MjFkMzczY2FkZTRlODMyNjI3YjRmNiIsImV4cCI6MTY4ODk1NjIyNiwidXNlcklkIjoiNDM3IiwiaWF0IjoxNjU3NDIwMjI2fQ.p4cX1iWn63FnYq5x2S7s-aj_ST41b_oamCUOGdAQNXA

```



使用后的日志格式：

```
2022-07-10 10:27:38.858  INFO 4888 --- [io-50004-exec-4] com.yuya.common.utils.JwtTokenUtils      121 : <0><10908077662149440> jwt过期时间：Mon Jul 10 10:27:38 CST 2023
2022-07-10 10:27:38.916  INFO 4888 --- [io-50004-exec-4] c.y.t.controller.UTeacherController      96 : <0><10908077662149440> [UTeacherController]======= 登录成功 =======，Token=Hzyy10 eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxNzMwNjgxODk3MiIsImF1ZCI6InJlc3RhcGl1c2VyIiwibmJmIjoxNjU3NDIwMDU4LCJpc3MiOiIwOThmNmJjZDQ2MjFkMzczY2FkZTRlODMyNjI3YjRmNiIsImV4cCI6MTY4ODk1NjA1OCwidXNlcklkIjoiNDM3IiwiaWF0IjoxNjU3NDIwMDU4fQ.3NNNnRZDw5_dn-wnqhuiSM4wu_J2ZQGLTap8bwJvpzk
```