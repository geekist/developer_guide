
## 使用Annotation注入依赖

使用Spring的IoC容器，实际上就是通过类似XML这样的配置文件，把我们自己的Bean的依赖关系描述出来，然后让容器来创建并装配Bean。一旦容器初始化完毕，我们就直接从容器中获取Bean使用它们。


使用XML配置的优点是所有的Bean都能一目了然地列出来，并通过配置注入能直观地看到每个Bean的依赖。它的缺点是写起来非常繁琐，每增加一个组件，就必须把新的Bean配置到XML中。


有没有其他更简单的配置方式呢？


有！我们可以使用Annotation配置，可以完全不需要XML，让Spring自动扫描Bean并组装它们。


我们把上一节的示例改造一下，先删除XML配置文件，然后，给UserService和MailService添加几个注解。

首先，我们给MailService添加一个@Component注解：

```java

@Component
public class UserService {
    ...
}
```

这个@Component注解就相当于定义了一个Bean，它有一个可选的名称，默认是userService，即小写开头的类名。

然后，我们给UserController添加一个@Component注解和一个@Autowired注解：

```java

@Component

public class UserController {

    @Autowired
    USerService userService;

    ...
}

```
使用@Autowired就相当于把指定类型的Bean注入到指定的字段中。

和XML配置相比，@Autowired大幅简化了注入，因为它不但可以写在set()方法上，还可以直接写在字段上，甚至可以写在构造方法中：

```
@Component
public class UserController {
    UserService userService;

    public UserController(@Autowired UserService userService) {
        this.userService = userService;
    }
    ...
}
```
我们一般把@Autowired写在字段上，通常使用package权限的字段，便于测试。

最后，编写一个AppConfig类启动容器：

```
@Configuration
@ComponentScan
public class AppConfig {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        UserController userController = context.getBean(UserController.class);
        User user = userService.login("bob@example.com", "password");
        System.out.println(user.getName());
    }
}
```

除了main()方法外，AppConfig标注了@Configuration，表示它是一个配置类，因为我们创建ApplicationContext时：

```
ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
```

使用的实现类是AnnotationConfigApplicationContext，必须传入一个标注了@Configuration的类名。

此外，AppConfig还标注了@ComponentScan，它告诉容器，自动搜索当前类所在的包以及子包，把所有标注为@Component的Bean自动创建出来，并根据@Autowired进行装配。

整个工程结构如下：

spring-ioc-annoconfig
├── pom.xml
└── src
    └── main
        └── java
            └── com
                └── itranswarp
                    └── learnjava
                        ├── AppConfig.java
                        └── service
                            ├── MailService.java
                            ├── User.java
                            └── UserService.java


- 使用Annotation配合自动扫描能大幅简化Spring的配置，我们只需要保证：
 
- 每个Bean被标注为@Component并正确使用@Autowired注入；
 
- 配置类被标注为@Configuration和@ComponentScan；
 
- 所有Bean均在指定包以及子包内。

使用@ComponentScan非常方便，但是，我们也要特别注意包的层次结构。通常来说，启动配置AppConfig位于自定义的顶层包（例如com.itranswarp.learnjava），其他Bean按类别放入子包。

[资源文件注入、配置注入和条件装配](./spring_ioc_4.md)