
## 一个最简单的java文件分析

```java


```java

import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.web.bind.annotation.*;
@RestController
@EnableAutoConfiguration
public class Example {
    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }
    public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }
}

```

### 1、main方法

main方法是一个标准方法，其遵循 Java 规范中定义的应用程序入口点。我们的 main 方法通过调用 run 来委托 Spring Boot 的 SpringApplication 类，SpringApplication 类将引导我们的应用，启动 Spring，然后启动自动配置的 Tomcat web 服务器。我们需要将 Example.class 作为一个参数传递给 run 方法来告知 SpringApplication，它是 Spring 主组件。同时还传递 args 数组以暴露所有命令行参数。

### 2、注解

* @RestController注解
@RestController注解被称作 stereotype 注解。它能为代码阅读者提供一些提示，对于 Spring 而言，这个类具有特殊作用。在本示例中，我们的类是一个 web @Controller，因此 Spring 在处理传入的 web 请求时会考虑它。

* @RequestMapping 注解提供了 routing（路由）信息。它告诉 Spring，任何具有路径为 / 的 HTTP 请求都应映射到 home 方法。@RestController 注解告知 Spring 渲染结果字符串直接返回给调用者。

注：

@RestController 和 @RequestMapping 是 Spring MVC 注解（它们不是 Spring Boot 特有的）。有关更多详细信息，请参阅 Spring 参考文档中的 MVC 章节


* @EnableAutoConfiguration 注解

第二个类级别注解是 @EnableAutoConfiguration。此注解告知 Spring Boot 根据您添加的 jar 依赖来“猜测”您想如何配置 Spring 并进行自动配置，由于 spring-boot-starter-web 添加了 Tomcat 和 Spring MVC，auto-configuration（自动配置）将假定您要开发 web 应用并相应设置了 Spring。

Starter 与自动配置 Auto-configuration 被设计与 Starter 配合使用，但这两个概念并不是直接相关的。您可以自由选择 starter 之外的 jar 依赖，Spring Boot 仍然会自动配置您的应用程序。