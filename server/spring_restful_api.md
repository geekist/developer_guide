
## 一、创建一个SpringBoot的Web项目

建立一个Spring Starter Project项目，将Spring Boot Starter Web依赖项添加到构建配置文件pom.xml(使用Marven构建)中：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

## 二、根据MVC的架构思想创建model和controller

### 2.1 @RestControl注解
```java

@Target(value=TYPE)  
 @Retention(value=RUNTIME)  
 @Documented  
 @Controller  
 @ResponseBody  
public @interface RestController  

```

@Controller来标识当前类是一个控制器servlet。

@ResponseBody用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。

### 2.2 @RequestMapping、@GetMapping、@PostMapping

@RequestMapping 配置url映射

@RequestMapping此注解即可以作用在控制器的某个方法上，也可以作用在此控制器类上。

当控制器在类级别上添加@RequestMapping注解时，这个注解会应用到控制器的所有处理器方法上。处理器方法上的@RequestMapping注解会对类级别上的@RequestMapping的声明进行补充。

例子一：@RequestMapping仅作用在处理器方法上

```java
@RestController
public class HelloController {

    @RequestMapping(value="/hello",method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
}
```

以上代码sayHello所响应的url=localhost:8080/hello。

例子二：@RequestMapping仅作用在类级别上


```java
@Controller
@RequestMapping("/hello")
public class HelloController {

    @RequestMapping(method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
}
```

以上代码sayHello所响应的url=localhost:8080/hello,效果与例子一一样，没有改变任何功能。

例子三：@RequestMapping作用在类级别和处理器方法上

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @RequestMapping(value="/sayHello",method= RequestMethod.GET)
    public String sayHello(){
        return "hello";
    }
    @RequestMapping(value="/sayHi",method= RequestMethod.GET)
    public String sayHi(){
        return "hi";
    }
}
```
这样，以上代码中的sayHello所响应的url=localhost:8080/hello/sayHello。

sayHi所响应的url=localhost:8080/hello/sayHi。

@GetMapping 是在 @RequestMapping 基础上的封装

```java
@Target({ java.lang.annotation.ElementType.METHOD })
@Retention(RetentionPolicy.RUNTIME)
@Documented
@RequestMapping(method = { RequestMethod.GET })
public @interface GetMapping {
 // abstract codes
 ...
}
```
Spring4.3中引进了｛@GetMapping、@PostMapping、@PutMapping、@DeleteMapping、@PatchMapping｝，来帮助简化常用的HTTP方法的映射，并更好地表达被注解方法的语义。




### 2.2 SpringBoot 工程目录

根目录：src.main.java

* 1.工程启动类(Application.java)：置于com.cy.project包下或者com.cy.project.app包下

* 2.实体类(domain)：置于com.cy.project.domain

* 3.数据访问层(Dao)：置于com.cy.project.repository（dao）

* 4.数据服务层(Service)：置于com.cy.project.service 

* 5.数据服务接口的实现(serviceImpl)：同样置于com.cy.project.service或者置于com.cy.project.service.impl

* 6.前端控制器(Controller)：置于com.cy.project.controller

* 7.工具类(utils)：置于com.cy.project.utils

* 8.常量接口类(constant)：置于com.cy.project.constant

* 9.配置信息类(config)：置于com.cy.project.config

资源文件：src.main.resources

* 1.页面以及js/css/image等置于static文件夹下的各自文件下

* 2.使用模版相关页面等置于templates文件夹下的各自文件下

