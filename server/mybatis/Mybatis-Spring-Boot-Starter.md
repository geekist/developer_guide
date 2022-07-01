# 一、MyBatis-Spring-Boot-Starter 简介

MyBatis-Spring-Boot-Starter 是 mybatis 为 springboot 提供的快速集成的方案（因为 springboot 太火了），原话是 The MyBatis-Spring-Boot-Starter help you build quickly MyBatis applications on top of the Spring Boot。因此如果项目中使用 springboot 和 mybatis 的话，这个 starter 可以大大的简化你的工作。

MyBatis-Spring-Boot-Starter类似一个中间件，链接Spring Boot和MyBatis，构建基于Spring Boot的MyBatis应用程序。

MyBatis-Spring-Boot-Starter 当前版本是 2.1.2，发布于2020年3月10日

MyBatis-Spring-Boot-Starter是个集成包，因此对MyBatis、MyBatis-Spring和SpringBoot的jar包都存在依赖，如下所示：

| MyBatis-Spring-Boot-Starter |	MyBatis-Spring | Spring Boot	| Java |
| ----  | ----  | ---- | ---- |
| 2.1	| 2.0 (need 2.0.2+ for enable all features)	| 2.1 or higher | 	8 or higher |
| 1.3	| 1.3 |	1.5	| 6 or higher |


# 二、 MyBatis-Spring-Boot-Starter 使用

## 添加依赖

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

## 配置数据源

```yml
spring:
  datasource:
    dynamic:
      primary: master #设置默认的数据源或者数据源组,默认值即为master
      datasource:
        master:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_biz?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        user:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_user?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        class:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_class?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        task:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_task?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        message:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_message?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
```


众所周知，MyBatis的核心有两大组件：SqlSessionFactory 和 Mapper 接口。前者表示数据库链接，后者表示SQL映射。当我们基于Spring使用MyBatis的时候，也要保证在Spring环境中能存在着两大组件。

## MyBatis-Spring-Boot-Starter功能

* 1、Autodetect an existing DataSource

自动发现存在的DataSource

* 2、Will create and register an instance of a SqlSessionFactory passing that DataSource as an input using the SqlSessionFactoryBean

利用SqlSessionFactoryBean创建并注册SqlSessionFactory

* 3、Will create and register an instance of a SqlSessionTemplate got out of the SqlSessionFactory

创建并注册SqlSessionTemplate

* 4、Auto-scan your mappers, link them to the SqlSessionTemplate and register them to Spring context so they can be injected into your beans

自动扫描Mappers，并注册到Spring上下文环境方便程序的注入使用