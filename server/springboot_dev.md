
## 开发第一个Spring Boot项目

### 1、环境配置

开发SpringBoot项目需要配置jdk和maven环境，具体参见：

* [jdk环境配置](http://www.51gjie.com/java/43.html)

* [maven环境配置](https://www.runoob.com/maven/maven-setup.html)

如果使用IDEA的spring initializer来开发，则maven配置是内置的。

* springboot环境配置

如果使用IDEA开发springboot项目，则直接可以用spring initializer创建一个spring boot的web项目。

## 2.用IDEA开发第一个SpringBoot项目

* [用IDEA创建一个Spring项目](https://blog.csdn.net/u012561176/article/details/91039237)

## 3.SpringBoot目录文档分析：

### 3.1 pom.xml文件分析

* [pom.xml分析](https://github.com/geekist/developer_guide/blob/main/server/pom.md)

### 3.2 主文件分析

* [主文件分析](https://github.com/geekist/developer_guide/blob/main/server/springboot-file.md)

## 4、编译和运行spring boot项目

* 方法1：
 
再IDEA环境下，直接右键选择运行即可。

* 方法2：

使用了 spring-boot-starter-parent POM，可以使用 run 来启动应用程序。在根目录下输入 mvn spring-boot:run 以启动应用：

在工程目录下，执行 **mvn spring-boot:run **

## 5、打包项目

- [Spring Boot项目打包成jar并在本地服务器运行](https://github.com/geekist/developer_guide/blob/main/server/spring_jar.md)

## 6、发布到远程服务器

- [Spring Boot在远程服务器运行](https://github.com/geekist/developer_guide/blob/main/server/spring_jar_server.md)



