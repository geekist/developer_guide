## Spring boot项目框架

## 1、用IDEA创建一个springboot项目

 - [Idea 环境下第一个Spring Boot项目](https://github.com/geekist/developer_guide/blob/main/server/springboot_dev.md)

## 2、pom.xml配置

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.yf.springboot</groupId>
	<artifactId>spring-boot-myfirst</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<!-- Inherit defaults from Spring Boot -->
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.4.1.RELEASE</version>
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<java-version>1.8</java-version>
	</properties>

	<!-- Add typical dependencies for a web application -->
	<dependencies>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
			<!-- 不需要version 会根据parent版本进行添加上 -->
		</dependency>
		<!-- 添加fastjson 支持 -->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.15</version>
		</dependency>
		
		<!-- 使用thymeleaf模板  -->
        <dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
       
         
         <!-- 使用freemaker模板 -->
        <dependency>
        	<groupId>org.springframework.boot</groupId>
        	<artifactId>spring-boot-starter-freemarker</artifactId>
        </dependency>


	</dependencies>

	<!-- Package as an executable jar -->
	<build>

		<plugins>

			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
				</configuration>
			</plugin>

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>

			<!-- 在这里添加springLoader plugin 实现热拔插开发 1、run as maven build 里面配置 goal: 
				spring-boot:run 问题控制台maven重启会报错：Verify the connector's configuration, identify 
				and stop any process that's listening on port 8080, or configure this application 
				to listen on another port. 一般不用这种。关闭还会占用端口 2、run as -java application 需要把springloaded-1.2.3.RELEASE.jar下载下来。放在工程目录下 
				然后run-config设置jvm 参数 -javaagent:.\lib\springloaded-1.2.3.RELEASE.jar -noverify -->
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<dependencies>
					<dependency>
						<groupId>org.springframework</groupId>
						<artifactId>springloaded</artifactId>
						<version>1.2.3.RELEASE</version>
					</dependency>
				</dependencies>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
						</goals>
					</execution>

				</executions>
				<configuration>
					<classifier>exec</classifier>
				</configuration>
			</plugin>

		</plugins>
	</build>

</project>  

```

## 3、项目配置文件application.yml

采用spring boot主要配置文件application.yml，建议使用装上spring suit插件会有提示，使用IDEA也可以有相应插件支持。
用yml做配置文件有严格格式和有提示支持，properties文件不太方便。
没有配置文件，启动的spring boot会默认使用8080端口。

```xml

server:
  port: 8800

spring:
  thymeleaf:
    prefix: classpath:/templates/
    suffix: .html
    mode: HTML5
    encoding: UTF-8
    content-type: text/html; charset=utf-8
#    设置缓存为false 为了热部署 host refresh
    cache: false
#    设置freemarker
  freemarker:
    allow-request-override: false
#    开发过程建议关闭缓存
    cache: true
    check-template-location: false
    charset: UTF-8
    content-type: text/html; charset=utf-8
    expose-request-attributes: false
    expose-session-attributes: false
    expose-spring-macro-helpers: false
#    prefix: xx
    request-context-attribute: 
#    settings: 
# 默认后缀就是.ftl
#    suffix: .ftl
#    template-loader-path: classPath:/templates/
#    view-names:
  
```

3、编写启动主类

注意：main类所在包是其他类的顶级包，这样才能能扫描到controller等注解。
注意：这里映入了第三方json处理依赖

```java
package com.yf.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.web.HttpMessageConverters;
import org.springframework.context.annotation.Bean;
import org.springframework.http.converter.HttpMessageConverter;

import com.alibaba.fastjson.serializer.SerializerFeature;
import com.alibaba.fastjson.support.config.FastJsonConfig;
import com.alibaba.fastjson.support.spring.FastJsonHttpMessageConverter;

/**
 * 出现404解决
 * 1、地址是否正确
 * 2、注解是否对了
 * 3、包路径是否正确，能否被扫描到加载到
 * 默认启动8080端口
 * @Description 这里使用@SpringBootApplication只是一个springBoot应用程序
 * @author Administrator
 * @date 2017-4-21 下午9:08:54 
 * @version V1.3.1
 */
@SpringBootApplication
public class Application  
//extends WebMvcConfigurerAdapter 
{
    /**
     * 添加第三方json工具
	 * 1、需要再pom.xml加入相关以来
	 * 2、需要再APP 继承 WeWebMvcConfigurerAdapter  重写configureMessageConverters
	 * 3、或者使用bean注入fastJsonHttpMessageConverters
	 * 
	 * 
	 * 配置fastjson支持两种方法
	 * 一：1、启动继承 WebMvcConfigurerAdapter 2、覆盖方法configureMessageConverters
	 * 二：使用bean注入fastJsonHttpMessageConverters
	 * 这里使用@Bean注入 HttpMessageConverters
	 * @Description 
	 * @author Administrator
	 * @return
	 */
	@Bean
	public HttpMessageConverters fastJsonHttpMessageConverters(){
		//1、定义convert转换消息对象
		FastJsonHttpMessageConverter fasConverter  = new  FastJsonHttpMessageConverter();
		//2、添加fastJson的配置信息，比如：是否要格式化返回json数据
		FastJsonConfig fastJsonConfig = new FastJsonConfig();
		fastJsonConfig.setSerializerFeatures(SerializerFeature.PrettyFormat);
		//3、再convert中添加配置信息
		fasConverter.setFastJsonConfig(fastJsonConfig);
		HttpMessageConverter<?> converter = fasConverter;
		return new HttpMessageConverters(converter); 
	}
    
	//	  1、需要先定义一个conver转换消息对象
	//	  2、添加fastJson配置信息，比如：食肉需要格式化返回json数据
	//	  3、再convert中添加配置信息
	//	  4、讲convert添加到converters中
	/*@Override
	public void configureMessageConverters(
			List<HttpMessageConverter<?>> converters) {
		super.configureMessageConverters(converters);
		FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();
		FastJsonConfig fastJsonConfig = new FastJsonConfig();
		fastJsonConfig.setSerializerFeatures(SerializerFeature.PrettyFormat);
		fastConverter.setFastJsonConfig(fastJsonConfig);
		converters.add(fastConverter);
	}*/

	public static void main ( String[] args )
    {
    	SpringApplication.run(Application.class, args);  
    }

}
```
4、controller类开发HTTP请求接口

```java

package com.yf.springboot.controller;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;
import com.yf.springboot.entity.User;
import com.yf.springboot.entity.User2;

/**
 * 注解全名：
 * @Description 使用RestController 相当于@Controller 和 @RequestBody
 * @author Administrator
 * @date 2017-4-21 下午9:06:28
 * @version V1.3.1
 */
// 相当于 @Controller + @ResponseBody
// 该注解 方法method 返回类型是String时候则返回string,返回对象时候则讲json_encode 该对象的json字符串
@RestController
// @EnableAutoConfiguration
@RequestMapping("/test")
public class HelloController {

	/**
	 * 请求： http://localhost:8800/test/ http方式：get 请求返回contentType: text/plain
	 * 请求responseBody: "Hello page"
	 * 
	 * @Description
	 * @author Administrator
	 * @return
	 */
	@RequestMapping("/")
	public String index() {
		return "Hello page";
	}

	/**
	 * 请求：http://localhost:8800/test/hello/ 
	 * http方式：get 请求
	 * 返回contentType: text/plain 
	 * 请求responseBody: "Hello 123 abc 123 123 !!!"
	 * @Description
	 * @author Administrator
	 * @return
	 */
	@RequestMapping("/hello")
	public String hello() {
		return "Hello 123 abc 123 123 !!!";
	}

	/**
	 * 请求：http://localhost:8800/test/hello/testname 
	 * 请求返回contentType:
	 * responseBody: "Hello testname!!!"
	 * @Description
	 * @author Administrator
	 * @param myName
	 * @return
	 */
	@RequestMapping("/hello3/{myName}")
	public String hello3(@PathVariable String myName) {
		return "Hello " + myName + "!!!";
	}

	/**
	 * 请求：http://localhost:8800/test/hello4?id=123&name=abc 
	 * http方式：get
	 * 请求返回contentType: text/plain 
	 * 请求responseBody: ""id:123 name:abc""
	 * @Description
	 * @author Administrator
	 * @param id
	 * @param name
	 * @return
	 */
	@RequestMapping("/hello4")
	public String hello3(int id, String name) {
		System.out.println("id:" + id + " name:" + name);
		return "id:" + id + " name:" + name;
	}

	/**
	 * 请求：http://localhost:8800/test//getDemo/testname 
	 * http方式：get
	 * 请求返回contentType: application/json 
	 * 请求responseBody: { "createTime": 1495640486000, "id": 123, "name": "good122223" } fastjson支持
	 * 返回对象则自动解析为json字符串 因为spring boot
	 * 默认使用json解析框架自动返回，返回头是Content-Type:application/json;charset=UTF-8
	 * @Description
	 * @author Administrator
	 * @param myName
	 * @return {"id":123,"name":"good","createTime":1492782569909}
	 */
	@RequestMapping("/getDemo/{myName}")
	User getDemo(@PathVariable String myName) {
		User user = new User();
		user.setId(123);
		user.setName("good122223");
		user.setCreateTime(new Date());
		System.out.println("Hello " + myName + "!!!");
		return user;
	}

	/**
	 * 请求：http://localhost:8800/test//getDemo/testname 
	 * http方式：get
	 * 请求返回contentType: application/json 
	 * 请求responseBody: { "createTime": "2017-05-24 23:43:02", "id": 123, "name": "sss" }
	 * spring boot 默认使用jackjson来解析json 使用fastjson返回json 因为spring boot
	 * 默认使用json解析框架自动返回， 返回头是Content-Type:application/json;charset=UTF-8
	 * @Description
	 * @author Administrator
	 * @param myName
	 * @return {"createTime":"2017-05-20 13:23:57","id":123,"name":"sss"}
	 */
	@RequestMapping("/getJson")
	public User2 getJson() {
		User2 user = new User2();
		user.setId(123);
		user.setName("sss");
		user.setCreateTime(new Date());
		return user;
	}

	/**
	 * 请求：http://localhost:8800/test//getMapping?id=123&name=abc 
	 * http方式：get
	 * 请求返回contentType: application/json 
	 * 请求responseBody: { "id": 123, "name": "abc" }
	 * @Description
	 * @param user
	 * @return
	 */
	// 只有get方式能成功,发送post会报错。@GetMapping = @RequestMapping(method = { RequestMethod.GET }
	@GetMapping("/getMapping")
	public User getMapping(User user) {
		return user;
	}

	/**
	 * 请求：http://localhost:8800/test//getMapping2?id=123&name=abc 
	 * http方式：get
	 * 请求返回contentType: application/json 
	 * 请求responseBody: { "id": 123, "name": "abc" } 
	 * 请求boty设置为 application/x-www-form-urlencoded 也可以接受参数 
	 * body : id=123&name=abc 
	 * @RequestMapping(value="/getMapping" ) 
	 * 等于 @RequestMapping(value="/getMapping" , method={RequestMethod.GET,RequestMethod.POST})
	 * @Description
	 * @param user
	 * @return
	 */
	@RequestMapping(value = "/getMapping2", method = { RequestMethod.GET, RequestMethod.POST })
	public User getMapping2(User user) {
		return user;
	}

	/**
	 * @RequestBody String 直接获取请求体， 不封装 
	 * 请求：http://localhost:8800/test/get2
	 * http方式：post
	 * {"createTime":1495640486000,"name":"good122223","id":123}
	 * 请求返回contentType: application/json 请求responseBody:
	 * {"createTime":1495640486000,"name":"good122223","id":123}
	 * @Description
	 * @param user
	 * @return
	 */
	@RequestMapping("/get2")
	public User get2(@RequestBody String reqContent, @RequestBody User user) {
		// {"name":"abc","id":123}
		System.out.println("reqContent:" + reqContent);
		System.out.println(user.toString());
		return user;
	}

	/**
	 * 对json数据进行实体类封装 请求：http://localhost:8800/test/user 
	 * http方式：post
	 * 请求requestBody:
	 * {"createTime":1495640486000,"name":"good122223","id":123}
	 * 请求返回contentType: application/json 
	 * 请求responseBody: {"createTime":1495640486000,"name":"good122223","id":123}
	 * @Description
	 * @param user
	 * @return
	 */
	// {"id":2,"username":"user2","name":"李四","age":20,"balance":100.00}
	// 发送格式没application/json
	@PostMapping("/user")
	public User postUser(@RequestBody User user) {
		System.out.println(user);
		return user;
	}

	/**
	 * 请求：http://localhost:8800/test/list-all 
	 * http方式：get 
	 * 请求返回contentType:
	 * 请求responseBody: [ { "createTime": 1495641815658, "id": 1, "name":
	 * "zhangsan" }, { "createTime": 1495641815658, "id": 1, "name": "zhangsan"
	 * }, { "createTime": 1495641815658, "id": 1, "name": "zhangsan" } ]
	 * 
	 * @Description
	 * @param user
	 * @return
	 */
	@GetMapping("list-all")
	public List<User> listAll() {
		ArrayList<User> list = new ArrayList<User>();
		User user = new User(1, "zhangsan", new Date());
		User user2 = new User(1, "zhangsan", new Date());
		User user3 = new User(1, "zhangsan", new Date());
		list.add(user);
		list.add(user2);
		list.add(user3);
		return list;

	}

	/**
	 * 会自动使用fastjson进行数据解析 
	 * 请求：http://localhost:8800/test/list-all2 
	 * http方式：get
	 * 请求responseBody: [ { "createTime": "2017-05-25 00:11:11",
	 * "id": 1, "name": "zhangsan" }, { "createTime": "2017-05-25 00:11:11",
	 * "id": 1, "name": "zhangsan" } ]
	 * 
	 * @Description
	 * @param user
	 * @return
	 */
	@GetMapping("list-all2")
	public List<User2> listAll2() {
		ArrayList<User2> list = new ArrayList<User2>();
		User2 user = new User2(1, "zhangsan", new Date(), "beijing");
		User2 user2 = new User2(1, "zhangsan", new Date(), "sh");
		list.add(user);
		list.add(user2);
		return list;

	}
}

```

5、controller开发模板请求接口测试类

```java

package com.yf.springboot.controller;

import java.util.Map;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;


/**
 * 1、thymeleaf末班文件中 标签是需要闭合的 3.0之前需要不和
 * 2、thymeleaf 3.0+ 是可以不强制要求闭合
 * 3、支持多个模板 比如thymeleaf 、freemarker可以并存
 * @Description 使用RestController 相当于@Controller 和 @RequestBody
 * @author Administrator
 * @date 2017-4-21 下午9:06:28 
 * @version V1.3.1
 */
//@Controller @ResponseBody
@Controller
@RequestMapping("/template")
public class TemplateController {

	/**
	 * javax.servlet.ServletException: Circular view path [hello]: would dispatch back to the current handler URL [/template/hello] again. Check your ViewResolver setup! (Hint: This may be the result of an unspecified view, due to default view name generation.)
	 * 
	 * 摘录答案中的话，When you don’t declare a ViewResolver, spring registers a default InternalResourceViewResolver which creates instances of JstlView for rendering the View. 
当你没有声明ViewResolver时，spring会给你注册一个默认的ViewResolver，其是JstlView的实例。它通过RequestDispatcher寻找资源（视图），不过这个资源也可能是Servlet，也就是说，Controller中方法返回字符串（视图名），也可能会解析成Servlet。当你的请求路径与视图名相同时，就会发生死循环。
	 * @Description 
	 * @author Administrator
	 * @return
	 */
	//报错this may be the result of an unspecified view, due to default view name generation.)] 
	@RequestMapping("/hello")  
    public String index() {  
        return "hello";  
    }
	/**
	 * org.thymeleaf.exceptions.TemplateInputException: Error resolving template "hello-1", template might not exist or might not be accessible by any of the configured Template Resolvers
	 * @Description 
	 * @author Administrator
	 * @return
	 */
	@RequestMapping("/hello2")  
    public String index2() {  
        return "hello1";  
    }
	
	@RequestMapping("/hello3")  
    public  ModelAndView hello3() { 
		ModelAndView modelAndView = new ModelAndView("hello1");
        return modelAndView;
//        return "hello1";   等价于 return modelAndView;
    }
	
	@RequestMapping("/hello4")  
    public  String hello4(Map<String,Object> map) { 
		map.put("name", "Andy");
		return "hello1";
    }
	//freemarker 模板
	@RequestMapping("/helloFtl")  
    public  String hello5() { 
		return "helloFtl";
    }
	
	//freemarker 模板
		@RequestMapping("/helloFtl2")  
	    public  String hello6(Map<String,Object> map) { 
			map.put("name", "Andy");
			return "helloFtl2";
	    }
	
	
}

```

对应实体类User和User2仅展示属性

```java

/**
 * 
 * @Description 测试实体类 
 * @author Administrator
 * @date 2017-4-21 下午9:14:27 
 * @version V1.3.1
 */
public class User {
	private int id;
	private String name;
	private Date createTime;
}


/**
 * 
 * @Description fastjson测试实体类 
 * @author Administrator
 * @date 2017-4-21 下午9:14:27 
 * @version V1.3.1
 */
public class User2 {
	private int id;
	private String name;
	@JSONField(format="yyyy-MM-dd HH:mm:ss")
	private Date createTime;
	
	//不出现
	@JSONField(serialize=false)
	private String remark;//备注信息
}
	
```
对应项目模板文件：放在src/main/resources 的templates下 在application.yml中可配置。上面已经给出：

hello1.html文件内容

```html
<!DOCTYPE html>
<html>
<head>
<!-- 不闭合会报错 -->
<!-- <meta charset="UTF-8"> -->
<!-- <meta charset="UTF-8" />-->
 <title>Insert title here</title>
</head>
<body>

	<h1>
	hello my thymeleaf
	<br/>
	this is my first thymeleaf demo
	</h1>
	welcome <p th:text="${name}"></p>
	welcome<span th:text="${name}"></span>
</body>
</html>
````
helloFtl.ftl文件内容

```html

<!DOCTYPE html>
<html>
<head>
<!-- 不闭合会报错 -->
<!-- <meta charset="UTF-8"> -->
<!-- <meta charset="UTF-8" />-->
 <title>Insert title here</title>
</head>
<body>
	<h1>
	hello my freemarker
	<br/>
	this is my first freemarker demo
	</h1>
</body>
</html>

```

helloFtl2 文件内容

```html
<!DOCTYPE html>
<html>
<head>
<!-- 不闭合会报错 -->
<!-- <meta charset="UTF-8"> -->
<!-- <meta charset="UTF-8" />-->
 <title>Insert title here</title>
</head>
<body>
	<h1>
	hello my freemarker
	<br/>
	this is my first freemarker demo
	</h1>
	name ${name}
</body>
</html>

```