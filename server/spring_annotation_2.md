
# Spring 注解


## Spring注解历史

* Spring1.x时代

在Spring1.x时代，还没出现注解，需要大量xml配置文件并在内部编写大量bean标签。


Java5推出新特性annotation，为spring的更新奠定了基础。

* Spring 2.X

从Spring 2.X开始spring将xml配置中的对象ioc过程转化成了注解。2.x时代Spring的注解体系有了雏形，属于“过渡时代”，引入了 @Autowired 、 @Controller 这一系列骨架式的注解。不过尚未完全替换XML配置驱动。

* Spring 3.x

Spring 3.x是里程碑式的时代，它引入了配置类@Configuration及@ComponentScan，使我们可以替换XML配置方式，全面拥抱Spring注解，在3.1抽象了一套全新的配置属性API，包括Environment和PropertySources这两个核心API。

* Spring4.X


Spring4.X是完善时代，趋于完善，引入了@Conditional（条件化注解），@Repeatable等。

* Spring5.x

当下是5.X时代，是SpringBoot2.0的底层核心框架，变化不大，引入了一个@Indexed注解，以提升应用启动性能的。


Spring Boot之所以能够轻松地实现应用的创建及与其他框架快速集成，最核心的原因就在于它极大地简化了项目的配置，最大化地实现了“约定大于配置”的原则。


## 替代xml和beans的Spring注解@Configuration和@Bean

### @Bean

Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。 产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。

@Bean注解作用于方法上，就是让方法去产生一个Bean，然后交给Spring容器。

例如：下面的类A中，accountDao方法产生一个AccountDao 对象，然后这个AccountDao 对象交给Spring管理

```
 class A{

     @Bean
      public AccountDao accountDao(){
          return new AccountDao();
      }
  }
```

实际上，@Bean注解和xml配置中的bean标签的作用是一样的。

***Q:用于注册Bean的注解的有那么多个，为何还要出现@Bean注解？***

@Component , @Repository , @ Controller , @Service 这些注册Bean的注解存在局限性，只能局限作用于自己编写的类，如果是一个jar包第三方库要加入IOC容器的话，这些注解就手无缚鸡之力了，是的，@Bean注解就可以做到这一点！当然除了@Bean注解能做到还有@Import也能把第三方库中的类实例交给spring管理，而且@Import更加方便快捷，只是@Import注解并不在本篇范围内，这里就不再概述。

使用@Bean注解的另一个好处就是能够动态获取一个Bean对象，能够根据环境不同得到不同的Bean对象。

@Bean注解总结

1、Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。 产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。

2、@Component , @Repository , @ Controller , @Service 这些注解只局限于自己编写的类，而@Bean注解能把第三方库中的类实例加入IOC容器中并交给spring管理。

3、@Bean注解的另一个好处就是能够动态获取一个Bean对象，能够根据环境不同得到不同的Bean对象。

4、@Bean就放在方法上，就是让方法去产生一个Bean，然后交给Spring容器，剩下的你就别管了。


### @Configuration

从Spring3.0，@Configuration用于定义配置类，可替换xml配置文件，等价于在XML中配置beans.被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。

注意：@Configuration注解的配置类有如下要求：

@Configuration不可以是final类型；
@Configuration不可以是匿名类；
嵌套的configuration必须是静态类。

```
/**
 * 说明：此处@Configuration 注解的作用，
 * 1、使配置类变成了full类型的配置类，spring在加载Appconfig的时候，Appconﬁg由普通类型转变为cglib代理类型 ，
 * 2、在 @Bean method中使用，是单例的，不会创建对个对象
 */
@ComponentScan("com.ytech")
@Configuration
public class AppConfig {

	@Bean
	public User user(){
		System.out.println("-----initMethod = \"user\"-return user -----");
		return new User();
	}
	
	@Bean
	public Cat cat(){
		return new Cat();
	}


	@Bean
	//条件注解，只有TestConditional返回为true时，才能实例化Fox
	@Conditional(value = TestConditional.class)
	public Fox fox(){
		//假如 Appconfig上使用了 @Configuration注解，cat()方法不会每次都返回一个新的cat 对象，而是返回一个公共的代理对象
		;
		System.out.println("test conditional");
		return new Fox(cat());
	}

````
使用@Configuration注解后，在调用方法 fox()创建 fox实例的时候，需要参数 cat，调用方法cat()生成cat实例，此时会去spring的单例bean工厂获取cat的单例bean的实例；

不使用@Configuration注解，实例化fox的时候，每次都会创建一个新的 cat对象，供实例化fox使用；

## @Component和其衍生的SpringMVC注解：@Controller、@Repository、@Service

### @Component注解：标准一个普通的spring Bean类。

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {
    String value() default "";
}
```

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个 Bean 不知道属于哪个层，可以使用`@Component` 注解标注。 后面三个注解都是被@Component标注的

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
    String value() default "";
}

```

- `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。

- `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao 层。

- `@Controller` : 对应 Spring MVC 控制层，主要用于接受用户请求并调用 Service 层返回数据给前端页面。

### 用@ComponentScan扫描bean

@ComponentScan主要就是定义扫描的路径从中找出标识了需要装配的类自动装配到spring的bean容器中

使用exclude filter来排除某些bean

```
@ComponentScan(value = "io.mieux",
        excludeFilters = {@Filter(type = FilterType.ANNOTATION,
        value = {Controller.class})})
public class BeanConfig {

}
```

### 注入依赖@Autowired和@qualifer

@Autowired

此注解用于bean的field、setter方法以及构造方法上，显式地声明依赖。根据type来autowiring。当在field上使用此注解，并且使用属性来传递值时，Spring会自动把值赋给此field。也可以将此注解用于私有属性(不推荐)，如下。

```
@Component
public class User {

@Autowired
private Address address;
}
```

@Qualifier

此注解是和@Autowired一起使用的。使用此注解可以让你对注入的过程有更多的控制。

@Qualifier可以被用在单个构造器或者方法的参数上。当上下文有几个相同类型的bean, 使用@Autowired则无法区分要绑定的bean，此时可以使用@Qualifier来指定名称。

```
@Component
public class User {

@Autowired
@Qualifier("address1")
private Address address;
...
}
```

@Required

此注解用于bean的setter方法上。表示此属性是必须的，必须在配置阶段注入，否则会抛出BeanInitializationExcepion。

### Spring核心注解按场景分类

1.模式注解

| Spring注解 |	场景说明	|起始版本 |
| ---- | ---- | ---- |
| @Component | 通用组件模式注解 | 2.5| 
| @Repository | 数据仓储模式注解| 2.0 |
| @Service | 服务模式注解 | 2.5
| @Controller | Web 控制器模式注解 | 2.5| 
| @Configuration | 配置类模式注解	 | 3.0| 

2.装配注解

| Spring注解 | 场景说明 | 起始版本 |
| ---- | ---- | ---- |
| @ImportResource |	替代 XML 元素 | 2.5 |
| @Import | 限定@Autowired 依赖注入范围 | 2.5 |
| @componentScan |扫描 指定 package 下标注spring 模式注解的类 | 3.1 |

3.依赖注入注解

| Spring注解	 | 场景说明 | 起始版本 |
| ---- | ---- | ---- |
| @Autowired | Bean 依赖注入，支持多种依赖查找方式 | 2.5 |
| @Qualifier |细粒度的@Autowired 依赖查找 |	2.5 |

| Java注解 | 场景说明  | 起始版本 |
| ---- | ---- | ---- |
| @Resouece	| Bean 依赖注入，仅支持名称依赖查找方式 | 2.5 |

4.Bean 自定义注解

|Spring注解 | 场景说明 | 起始版本 | 
| ---- | ---- | ---- |
| @Bean | 替代 XML 元素<bean> | 3.0 |
|@DependsOn | 替代 XML 属性<bean depends-on="..."/> | 3.0 | 
|@Lazy | 替代 XML 属性<bean lazy0init="true|falses"/> | 3.0 | 
|@Primary | 替代 XML 元素<bean primary="true|false"/> | 3.0 | 
|@Role | 替代 SML 元素<bean role="..."/>	 | 3.1 | 
|@Lookup | 替代 XML 属性<bean lookup-method="..."> | 4.1 | 

5.条件装配注解

|Spring注解	| 场景说明 | 起始版本 |
| ---- | ---- | ---- |
|@Profile | 配置化条件装配 | 3.1 | 
|@Conditional | 编程条件装配 | 3.1 | 

配置属性注解
| Spring注解 | 场景说明 | 起始版本
| ---- | ---- | ---- |
|@PropertySource | 配置属性抽象 PropertySource | 3.1 | 
|@PropertySources | @PropertySource集合注解 | 4.0 | 

生命周期回调注解

Spring注解 | 场景说明 | 	起始版本 | 
| ---- | ---- | ---- |
|@PostConstruct | 替换 XML 元素<bean init-method="..."/>或 InitializingBean | 2.5 | 
|@PreDestroy | 替换 XML 元素<bean destroy-method="..." />或 DisposableBean | 2.5 | 

注解属性注解
|Spring注解 | 场景说明 | 起始版本 | 
| ---- | ---- | ---- |
|@AliasFor | 别名注解属，实现复用的目的 | 4.2 | 

性能注解
|Spring注解 | 场景说明 | 起始版本 | 
| ---- | ---- | ---- |
|@Indexed | 提升 spring 模式注解的扫描效率 | 5.0 | 




















## @EnableScheduling 开启对定时任务的支持

## @Scheduled 可以作为一个触发源添加到一个方法中

   其中Scheduled注解中有以下几个参数：

　　1.cron是设置定时执行的表达式，如 0 0/5 * * * ?每隔五分钟执行一次 秒 分 时 天 月

　　2.zone表示执行时间的时区

　　3.fixedDelay 和fixedDelayString 表示一个固定延迟时间执行，上个任务完成后，延迟多长时间执行

　　4.fixedRate 和fixedRateString表示一个固定频率执行，上个任务开始后，多长时间后开始执行

　　5.initialDelay 和initialDelayString表示一个初始延迟时间，第一次被调用前延迟的时间














