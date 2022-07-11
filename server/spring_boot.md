
# 一、Sping boot介绍

## Spring Boot

Spring 是诞生于2002年的 Java 开发框架，可以说已经成为 Java 开发的事实标准。所谓事实标准就是虽然 Java 官方没有说它就是开发标准，但是在当前 Java 开发的众多项目中，当我们谈到产品级的 Java 项目的时候，大多都是基于 Spring 或者应用了 Spring 特性的。

Spring 基于 IOC 和 AOP 两个特性对 Java 开发本身进行了大大的简化。但是一个大型的项目需要集成很多其他组件，比如一个 WEB 项目，至少要集成 MVC 框架、Tomcat 这种 WEB 容器、日志框架、ORM框架，连接数据库要选择连接池吧……使用 Spring 的话每集成一个组件都要去先写它的配置文件，比较繁琐且容易出错。

Spring Boot 是由 Pivotal 团队提供的全新框架，2014 年 4 月发布 Spring Boot 1.0 2018 年 3 月 Spring Boot 2.0发布。它是对spring的进一步封装，其设计目的是用来简化 Spring 应用的初始搭建以及开发过程。怎么简化的呢？就是通过封装、抽象、提供默认配置等方式让我们更容易使用。

SpringBoot 基于 Spring 开发。SpringBoot 本身并不提供 Spring 框架的核心特性以及扩展功能，也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。

关于 SpringBoot 有一句很出名的话就是约定大于配置。采用 Spring Boot 可以大大的简化开发模式，它集成了大量常用的第三方库配置，所有你想集成的常用框架，它都有对应的组件支持，例如 Redis、MongoDB、Jpa、kafka，Hakira 等等。SpringBoot 应用中这些第三方库几乎可以零配置地开箱即用，大部分的 SpringBoot 应用都只需要非常少量的配置代码，开发者能够更加专注于业务逻辑。

## Spring boot特点

快速创建独立运行的Spring项目以及与主流框架集成

使用嵌入式的Servlet容器，应用无需打成WAR包

Starters自动依赖与版本控制

大量的自动配置，简化开发，也可修改默认值

无需配置XML，无代码生成，开箱即用

准生产环境的运行时应用监控

与云计算的天然集成

# 二、创建一个Spring boot项目

## Spring initializer

SpringBoot 官方推荐的构建应用的方式是使用 Spring Initializr，直接在网页上选择好构建工具、语言、SpringBoot 版本，填好自己的项目名和初始依赖，然后点Generate 按钮，就能下载一个构建好的工程的zip包，只需要把这个包解压之后导入IDE就可以了。

这已经是一个包含依赖的、完整的、可独立运行的springboot应用了！你所需要做的就是往里面填充自己的业务代码！

如果使用的是 IDEA 商业版的话，新建工程的时候直接有 Spring 的选项；如果是IDEA社区版的话，可以安装 Spring Assistant 这个插件可以实现同样的功能。它们的原理是帮你把连接 Spring Initializr 并下载解压这个过程自动化了，所以只需要保持网络畅通就行了。

## 编写最简单的Spring boot代码

```java
package com.springboot.test;
 
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
 
@RestController
@SpringBootApplication
public class TestApplication {
 
    @RequestMapping("/hello")
    public String index(){
        return "Hello World， Spring boot is good";
    }
 
    public static void main(String[] args) {
        SpringApplication.run(TestApplication.class, args);
    }
}
```

## 运行

在本地运行idea项目，然后再浏览器输入：

```
http://localhost:8080/hello

```

# 三、依赖分析：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <groupId>com.ytech</groupId>
    <artifactId>spring_boot_web_demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>spring_boot_web_demo</name>
    <description>spring_boot_web_demo</description>

    <properties>
        <java.version>1.8</java.version>
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