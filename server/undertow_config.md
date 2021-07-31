
## 基础配置

### 一、启用配置文件

* 在 src/main/resources 目录下面创建 undertow.txt 文件，该文件会被 jfinal undertow 自动加载并对 jfinal undertow 进行配置。

* 如果不想使用 "undertow.txt" 这个文件名，还可以通过 UndertowServer.create(AppConfig.class, "other.txt") 方法的第二个参数来指定自己喜欢的文件名。

* 在config文件的main函数中配置端口和开发模式

```java

public static void main(String[] args) {
        UndertowServer.start(AppConfig.class,80,true);
    }

```


配置文件创建好以后，就可以按照下面的文档来配置相应的功能了。

### 二、常用配置
```xml
# true 值支持热加载
undertow.devMode=true
undertow.port=80
undertow.host=0.0.0.0
 
# 绝大部分情况不建议配置 context path
undertow.contextPath=/abc

```

