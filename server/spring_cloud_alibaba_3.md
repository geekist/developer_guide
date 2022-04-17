
## SpringCloud Gateway 简介

SpringCloud Gateway 是 Spring Cloud 的一个全新项目，该项目是基于 Spring 5.0，Spring Boot 2.0 和 Project Reactor 等技术开发的网关，它旨在为微服务架构提供一种简单有效的统一的 API 路由管理方式。

SpringCloud Gateway 作为 Spring Cloud 生态系统中的网关，目标是替代 Zuul，在Spring Cloud 2.0以上版本中，没有对新版本的Zuul 2.0以上最新高性能版本进行集成，仍然还是使用的Zuul 2.0之前的非Reactor模式的老版本。而为了提升网关的性能，SpringCloud Gateway是基于WebFlux框架实现的，而WebFlux框架底层则使用了高性能的Reactor模式通信框架Netty。

Spring Cloud Gateway 的目标，不仅提供统一的路由方式，并且基于 Filter 链的方式提供了网关基本的功能，例如：安全，监控/指标，和限流。

提前声明：Spring Cloud Gateway 底层使用了高性能的通信框架Netty。

## SpringCloud Gateway 特征

SpringCloud官方，对SpringCloud Gateway 特征介绍如下：

（1）基于 Spring Framework 5，Project Reactor 和 Spring Boot 2.0

（2）集成 Hystrix 断路器

（3）集成 Spring Cloud DiscoveryClient

（4）Predicates 和 Filters 作用于特定路由，易于编写的 Predicates 和 Filters

（5）具备一些网关的高级功能：动态路由、限流、路径重写

从以上的特征来说，和Zuul的特征差别不大。SpringCloud Gateway和Zuul主要的区别，还是在底层的通信框架上。

简单说明一下上文中的三个术语：

（1）Filter（过滤器）：

和Zuul的过滤器在概念上类似，可以使用它拦截和修改请求，并且对上游的响应，进行二次处理。过滤器为org.springframework.cloud.gateway.filter.GatewayFilter类的实例。

（2）Route（路由）：

网关配置的基本组成模块，和Zuul的路由配置模块类似。一个Route模块由一个 ID，一个目标 URI，一组断言和一组过滤器定义。如果断言为真，则路由匹配，目标URI会被访问。

（3）Predicate（断言）：

这是一个 Java 8 的 Predicate，可以使用它来匹配来自 HTTP 请求的任何内容，例如 headers 或参数。断言的输入类型是一个 ServerWebExchange。

## 在项目中引入Sprint Cloud Gateway

### 新建一个工程，并引入Gateway的依赖

```
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>

```

### 将自己注册为一个spring coloud的服务

```
@EnableDiscoveryClient
@SpringBootApplication
public class YychildrenGatewayApplication {

    public static void main(String[] args) {
        SpringApplication.run(YychildrenGatewayApplication.class, args);
    }

}

```

### 在配置文件中进行配置

```

server:
  port: 10061

spring:

 application:
    name: yychildren-gateway

  cloud:

    nacos:
      discovery:
        server-addr: @nacos-addr@
        service:  ${spring.application.name} 

    gateway:
      - id: yychildren-console
          uri: lb://yychildren-console  #
          predicates:
            - Path=/api/console/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
            
        - id: yychildren-teacher
          uri: lb://yychildren-teacher
          predicates:
            - Path=/api/teacher/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1


        - id: yychildren-parent
          uri: lb://yychildren-parent
          predicates:
            - Path=/api/parent/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1


        - id: yychildren-core
          uri: lb://yychildren-core
          predicates:
            - Path=/api/core/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1
```

下面对一个路由规则做简单的说明：

```
 - id: yychildren-teacher
          uri: lb://yychildren-teacher
          predicates:
            - Path=/api/teacher/**
          filters:
            - SwaggerHeaderFilter
            - StripPrefix=1

```

* id

路由 id,没有固定规则，但唯一，建议与服务名对应

* uri: lb://yychildren-teacher

lb：uri 的协议，表示开启 Spring Cloud Gateway 的负载均衡功能。

service-name：服务名，Spring Cloud Gateway 会根据它获取到具体的微服务地址。

predicates:
     - Path=/api/teacher/**

条件断言，满足网关网址,例如：http://192.168.3.16：10061/api/teacher/**的所有请求，跳转到服务yychild-teacher下。



