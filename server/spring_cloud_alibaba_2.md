
## 创建一个项目作为父工程，并添加spring cloud和spring cloud alibaba的依赖


* 创建一个Maven项目，定义groupid和arifactId。

![](./assets/spring_cloud_alibaba_2_1.png)

* 创建好后，删除项目的src目录

![](./assets/spring_cloud_alibaba_2_2.png)

* 将pom.xml的packaging标签设置为：pom

```xml
<modelVersion>4.0.0</modelVersion>
<groupId>com.ytech</groupId>
<artifactId>warrior</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>
<name>warrior</name>
```

* 添加spring cloud的依赖，完成后的pom.xml文件如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>
    <groupId>com.ytech</groupId>
    <artifactId>warrior</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    <name>warrior</name>

    <modules>
        <module>common</module>
        <module>wolf_warrior</module>
    </modules>

    <properties>
        <java.version>1.8</java.version>
        <spring-boot.version>2.4.4</spring-boot.version>
        <spring-cloud.version>2020.0.2</spring-cloud.version>
        <spring-cloud-alibaba.version>2020.0.RC1</spring-cloud-alibaba.version>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>


    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-dependencies</artifactId>
                <version>${spring-cloud.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>${spring-cloud-alibaba.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

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

    <pluginRepositories>
        <!--
        <pluginRepository>
            <id>thirdparty</id>
            <url>http://yuya.51vip.biz:8081/nexus/content/repositories/thirdparty</url>
        </pluginRepository>
        -->
    </pluginRepositories>
</project>





```