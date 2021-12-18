
# Spring 注解


从广义上Spring注解可以分为两类：

* 一类注解是用于注册Bean

假如IOC容器就是一间空屋子，首先这间空屋子啥都没有，我们要吃大餐，我们就要从外部搬运食材和餐具进来。这里把某一样食材或者某一样餐具搬进空屋子的操作就相当于每个注册Bean的注解作用类似。注册Bean的注解作用就是往IOC容器中放（注册）东西！

用于注册Bean的注解： 比如@Component , @Repository , @Controller , @Service , @Configration这些注解就是用于注册Bean，放进IOC容器中，一来交给spring管理方便解耦，二来还可以进行二次使用，啥是二次使用呢？这里的二次使用可以理解为：在你开始从外部搬运食材和餐具进空屋子的时候，一次性搬运了猪肉、羊肉、铁勺、筷子四样东西，这个时候你要开始吃大餐，首先你吃东西的时候肯定要用筷子或者铁勺，别说你手抓，只要你需要，你就会去找，这个时候发现你已经把筷子或者铁勺放进了屋子，你就不用再去外部拿筷子进屋子了，意思就是IOC容器中已经存在，就可以只要拿去用，而不必再去注册！而拿屋子里已有的东西的操作就是下面要讲的用于使用Bean的注解！

1、@controller 控制器（注入服务）
用于标注控制层，相当于struts中的action层

2、@service 服务（注入dao）
用于标注服务层，主要用来进行业务的逻辑处理

3、@repository（实现dao访问）
用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件.

4、@component （把普通pojo实例化到spring容器中，相当于配置文件中的 
<bean id="" class=""/>）
泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类。

说明： 
<context:component-scan base-package=”com.*”> 
上面的这个例子是引入Component组件的例子，其中base-package表示为需要扫描的所有子包。 
共同点：被@controller 、@service、@repository 、@component 注解的类，都会把这些类纳入进spring容器中进行管理

一* 类注解是用于使用Bean

用于使用Bean的注解：比如@Autowired , @Resource注解，这些注解就是把屋子里的东西自己拿来用，如果你要拿，前提一定是屋子（IOC）里有的，不然就会报错，比如你要做一道牛肉拼盘需要五头牛做原材料才行，你现在锅里只有四头牛，这个时候你知道，自己往屋子里搬过五头牛，这个时候就直接把屋子里的那头牛直接放进锅里，完成牛肉拼盘的组装。是的这些注解就是需要啥想要啥，只要容器中有就往容器中拿！而这些注解又有各自的区别，比如@Autowired用在筷子上，这筷子你可能只想用木质的，或许只想用铁质的，@Autowired作用在什么属性的筷子就那什么筷子，而@Resource如果用在安格斯牛肉上面，就指定要名字就是安格斯牛肉的牛肉。


## @Bean注解概述

本篇文章主要讲的是@Bean注解，这个注解属于用于注册Bean的注解。

下面这段话部分摘自Spring中为什么要有@Bean注解？

Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。 产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。@Bean明确地指示了一种方法，什么方法呢？产生一个bean的方法，并且交给Spring容器管理；从这我们就明白了为啥@Bean是放在方法的注释上了，因为它很明确地告诉被注释的方法，你给我产生一个Bean，然后交给Spring容器，剩下的你就别管了。记住，@Bean就放在方法上，就是让方法去产生一个Bean，然后交给Spring容器。



如下就能让accountDao方法产生一个AccountDao 对象，然后这个AccountDao 对象交给Spring管理

```
 class A{
        @Bean
        public AccountDao accountDao(){
            return new AccountDao();
        }
    }
```

实际上，@Bean注解和xml配置中的bean标签的作用是一样的。

为什么要有@Bean注解？

不知道大家有没有想过，用于注册Bean的注解的有那么多个，为何还要出现@Bean注解？

原因很简单：类似@Component , @Repository , @ Controller , @Service 这些注册Bean的注解存在局限性，只能局限作用于自己编写的类，如果是一个jar包第三方库要加入IOC容器的话，这些注解就手无缚鸡之力了，是的，@Bean注解就可以做到这一点！当然除了@Bean注解能做到还有@Import也能把第三方库中的类实例交给spring管理，而且@Import更加方便快捷，只是@Import注解并不在本篇范围内，这里就不再概述。

使用@Bean注解的另一个好处就是能够动态获取一个Bean对象，能够根据环境不同得到不同的Bean对象。

@Bean注解总结

1、Spring的@Bean注解用于告诉方法，产生一个Bean对象，然后这个Bean对象交给Spring管理。 产生这个Bean对象的方法Spring只会调用一次，随后这个Spring将会将这个Bean对象放在自己的IOC容器中。

2、@Component , @Repository , @ Controller , @Service 这些注解只局限于自己编写的类，而@Bean注解能把第三方库中的类实例加入IOC容器中并交给spring管理。

3、@Bean注解的另一个好处就是能够动态获取一个Bean对象，能够根据环境不同得到不同的Bean对象。

4、、记住，@Bean就放在方法上，就是让方法去产生一个Bean，然后交给Spring容器，剩下的你就别管了。


## @Configuration


从Spring3.0，@Configuration用于定义配置类，可替换xml配置文件，被注解的类内部包含有一个或多个被@Bean注解的方法，这些方法将会被AnnotationConfigApplicationContext或AnnotationConfigWebApplicationContext类进行扫描，并用于构建bean定义，初始化Spring容器。

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
@ComponentScan("com.jiagouedu")
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


## @EnableScheduling 开启对定时任务的支持

## @Scheduled 可以作为一个触发源添加到一个方法中

   其中Scheduled注解中有以下几个参数：

　　1.cron是设置定时执行的表达式，如 0 0/5 * * * ?每隔五分钟执行一次 秒 分 时 天 月

　　2.zone表示执行时间的时区

　　3.fixedDelay 和fixedDelayString 表示一个固定延迟时间执行，上个任务完成后，延迟多长时间执行

　　4.fixedRate 和fixedRateString表示一个固定频率执行，上个任务开始后，多长时间后开始执行

　　5.initialDelay 和initialDelayString表示一个初始延迟时间，第一次被调用前延迟的时间














