

## 1、创建一个IDEA项目

## 2、添加Jfinal和jfinal-undertow依赖

```xml
<dependency>
    <groupId>com.jfinal</groupId>
    <artifactId>jfinal-undertow</artifactId>
    <version>2.5</version>
</dependency>
 
<dependency>
    <groupId>com.jfinal</groupId>
    <artifactId>jfinal</artifactId>
    <version>4.9.15</version>
</dependency>

<dependency>
    <groupId>io.undertow</groupId>
    <artifactId>undertow-websockets-jsr</artifactId>
    <version>2.0.34.Final</version>
</dependency>

```

## 3、添加congfig类，并实现main方法

```java
import com.jfinal.config.*;
import com.jfinal.server.undertow.UndertowServer;
import com.jfinal.template.Engine;

public class AppConfig extends JFinalConfig {

    public static void main(String[] args) {
        UndertowServer.start(AppConfig.class);
    }

    public void configConstant(Constants me) {
        me.setDevMode(true);
    }

    public void configRoute(Routes me) {
        // 使用路由扫描，参数 "demo." 表示只扫描 demo 包及其子包下的路由
        me.scan("controller.");
    }

    public void configEngine(Engine me) {
    }

    public void configPlugin(Plugins me) {
    }

    public void configInterceptor(Interceptors me) {
    }

    public void configHandler(Handlers me) {
    }
}

```

## 4、添加controller文件
```java

import com.jfinal.core.Controller;
import com.jfinal.core.Path;

@Path("/hello")
public class HelloController extends Controller {
    public void index() {
        renderText("Hello JFinal World.");
    }
}


```

## 5、启动项目

在Config 类文件上点击鼠标右键，选择 Debug As，再选择 Java Applidation 即可运行


## 6、开启浏览器看效果

打开浏览器在地址栏中输入: http://localhost/hello，输出内容为Hello JFinal World证明项目框架搭建完成



