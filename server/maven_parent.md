
## 创建一个Spring maven项目的父类模板

### step1： 创建一个spring initailizer的项目

### step2：修改该项目的pom.xml文件，将其改造成一个spring boot的父类项目

* ***parent标签***

```xml

  <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

```

* ***项目描述标签***

```xml

 <groupId>com.ytech</groupId>
 <artifactId>razor</artifactId>
 <version>0.0.1-SNAPSHOT</version>
 <name>razor</name>
 <packaging>pom</packaging>
 <description>Demo project for Spring Boot</description>

```

* ***属性标签***

```xml
 <properties>
        <java.version>1.8</java.version>
 </properties>

```

* ***子模块标签***

```xml

<modules>
        <module>simple</module>
        <module>curd</module>
        <module>druidonesource</module>
        <module>druidtwosource</module>
        <module>relatedoperation</module>
    </modules>

```

* ***依赖管理***

```xml
 <dependencyManagement>

        <dependencies>

            <!--spring boot starter 和 spring boot starter test-->
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter</artifactId>
            </dependency>

            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-test</artifactId>
                <scope>test</scope>
            </dependency>

            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>1.16.16</version>
            </dependency>

            <dependency>
                <groupId>org.mybatis.spring.boot</groupId>
                <artifactId>mybatis-spring-boot-starter</artifactId>
                <version>2.1.3</version>
            </dependency>

            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger2</artifactId>
                <version>2.5.0</version>
            </dependency>
          
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid-spring-boot-starter</artifactId>
                <version>1.1.17</version>
            </dependency>

        </dependencies>

    </dependencyManagement>


```

* ***repositories依赖的地址***

```xml
    <repositories>
        <repository>
            <id>aliyun_central</id>
            <url>https://maven.aliyun.com/repository/central</url>
        </repository>
        <repository>
            <id>aliyun_public</id>
            <url>https://maven.aliyun.com/repository/public</url>
        </repository>
    </repositories>
```




* ***build 标签***

```xml
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

```

### step3： 创建一个module继承父类

* ***parent标签***

```xml
 <parent>
        <groupId>com.ytech</groupId>
        <artifactId>razor</artifactId>
        <version>0.0.1-SNAPSHOT</version>
        <relativePath>../pom.xml</relativePath>
    </parent>

```

* ***项目描述标签***

```xml
<groupId>com.ytech</groupId>
    <artifactId>mybatis</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>mybatis</name>
    <description>Demo project for Spring Boot</description>
```

* ***属性标签***

```xml
 <properties>
        <java.version>1.8</java.version>
 </properties>

```


* ***依赖***

```xml

 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- Mybatis -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!-- Mysql驱动包 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
        </dependency>

        <!--阿里数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
        </dependency>

    </dependencies>


```

* ***build 标签***

```xml
 <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```





