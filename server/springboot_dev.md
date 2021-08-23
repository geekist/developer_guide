
## 开发第一个Spring Boot项目

### 1、环境配置

开发SpringBoot项目需要配置jdk和maven环境，具体参见：

* jdk


* maven

如果使用IDEA的spring initializer来开发，则maven配置是内置的。

## 2.用IDEA开发第一个SpringBoot项目

* [用IDEA创建一个Spring项目](https://blog.csdn.net/u012561176/article/details/91039237)

## 3.SpringBoot目录文档分析：

### 3.1 pom.xml文件分析

* [pom.xml分析](https://github.com/geekist/developer_guide/blob/main/server/pom.md)

### 3.2 主文件分析

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


