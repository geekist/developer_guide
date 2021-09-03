## Spring boot 项目构架

## 1、用IDEA创建一个springboot项目


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

## 

采用spring boot主要配置文件application.yml，建议使用装上spring suit插件会有提示，使用IDEA也可以有相应插件支持。
用yml做配置文件有严格格式和有提示支持，properties文件不太方便。
没有配置文件，启动的spring boot会默认使用8080端口。
