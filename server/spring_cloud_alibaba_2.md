
本文来学习 Spring Cloud Alibaba 提供的 Spring Cloud Alibaba Nacos Discovery 组件，基于 Spring Cloud 的编程模型，接入 Nacos 作为注册中心，实现服务的注册与发现。


## Nacos discovery服务注册与发现

服务发现是微服务架构体系中最关键的组件之一。如果尝试着用手动的方式来给每一个客户端来配置所有服务提供者的服务列表是一件非常困难的事，而且也不利于服务的动态扩缩容。

Nacos Discovery 可以帮助您将服务自动注册到 Nacos 服务端并且能够动态感知和刷新某个服务实例的服务列表。

除此之外，Nacos Discovery 也将服务实例自身的一些元数据信息-例如 host，port, 健康检查URL，主页等内容注册到 Nacos。

在使用注册中心时，一共有三种角色：服务提供者（Service Provider）、服务消费者（Service Consumer）、注册中心（Registry）。

>在一些文章中，服务提供者被称为 Server，服务消费者被称为 Client。

**Provider：**

- 启动时，向 Registry 注册自己为一个服务（Service）的实例（Instance）。

- 同时，定期向 Registry 发送心跳，告诉自己还存活。

- 关闭时，向 Registry 取消注册。

**Consumer：**

- 启动时，向 Registry 订阅使用到的服务，并缓存服务的实例列表在内存中。

- 后续，Consumer 向对应服务的 Provider 发起调用时，从内存中的该服务的实例列表选择一个，进行远程调用。

- 关闭时，向 Registry 取消订阅。

**Registry：**

- Provider 超过一定时间未心跳时，从服务的实例列表移除。

- 服务的实例列表发生变化（新增或者移除）时，通知订阅该服务的 Consumer，从而让 Consumer 能够刷新本地缓存。

- 当然，不同的注册中心可能在实现原理上会略有差异。例如说，Eureka 注册中心，并不提供通知功能，而是 Eureka Client 自己定期轮询，实现本地缓存的更新。


另外，Provider 和 Consumer 是角色上的定义，一个服务同时即可以是 Provider 也可以作为 Consumer。例如说，优惠劵服务可以给订单服务提供接口，同时又调用用户服务提供的接口。

## 搭建服务提供者，并注册到Nacos中

### 引入依赖

```xml
 <!--
        引入 Spring Boot、Spring Cloud、Spring Cloud Alibaba 三者 BOM 文件，进行依赖版本的管理，防止不兼容。
        在 https://dwz.cn/mcLIfNKt 文章中，Spring Cloud Alibaba 开发团队推荐了三者的依赖关系
     -->

    <properties>
        <java.version>1.8</java.version>
        <spring.cloud.version>Hoxton.SR3</spring.cloud.version>
        <spring.cloud.alibaba.version>2.2.0.RELEASE</spring.cloud.alibaba.version>
    </properties>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-parent</artifactId>
                <version>${spring.boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring.cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring.cloud.alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <dependencies>
        <!-- 引入 SpringMVC 相关依赖，并实现对其的自动配置 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- 引入 Spring Cloud Alibaba Nacos Discovery 相关依赖，将 Nacos 作为注册中心，并实现对其的自动配置 -->
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
        </dependency>
    </dependencies>
```

### 配置application.yaml文件，设置discovery

```
spring:
  application:
    name: spring-cloud-provider # Spring 应用名
  cloud:
    nacos:
      # Nacos 作为配置中心的配置项 对应NacosDiscoveryProperties 配置类
      discovery:
        server-addr: 127.0.0.1:8848 # Nacos 服务器地址
        service: ${spring.application.name} # 注册到 Nacos 的服务名，默认 ${spring.application.name}?

server:
  port: 18080 # 服务器端口 8080

```

### 在app文件中设置enablediscovery

```
@SpringBootApplication
@EnableDiscoveryClient
public class SpringCloudProviderApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringCloudProviderApplication.class, args);
    }

}
```

### 添加一个restful的controller

```
@RestController
public class ProviderController {

    @Autowired
    ProviderService providerService;


    @RequestMapping("/api/echo")
    public String echo(){
        return providerService.echo("just a test");
    }
}

```

将nacos运行起来，然后可以看到ProviderController已经被注册到nacos中了

