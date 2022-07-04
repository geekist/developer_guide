
# 一、功能

Mybatis Generator官方插件的一个插件，可以在Mybatis Generator生成的Entity类中加入lombok注解。

github地址：https://github.com/GuoGuiRong/mybatis-generator-lombok-plugin

# 二、使用

## 安装插件到本地

首先从服务器下载代码到本地
```shell
git clone https://github.com/GuoGuiRong/mybatis-generator-lombok-plugin.git
```

然后将插件编译安装到本地maven库
```shell
mvn clean install
```

## 将插件添加到MybatisGenerator的依赖中

```xml
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
        <overwrite>true</overwrite>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>com.chrm</groupId>
            <artifactId>mybatis-generator-lombok-plugin</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
    </dependencies>
</plugin>
```

## 在goneratorConfig配置文件中添加lombok插件说明

```xml
<!-- 整合lombok-->
<plugin type="com.chrm.mybatis.generator.plugins.LombokPlugin" >
	<property name="hasLombok" value="true"/>
</plugin>
```

完成的配置文件例子如下所示：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
		PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
		"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
	<classPathEntry
			location="E:/Maven/LocalRepository/mysql/mysql-connector-java/5.1.34/mysql-connector-java-5.1.34.jar" />
	<context id="MysqlTables" targetRuntime="MyBatis3" defaultModelType="flat">

		<property name="javaFileEncoding" value="UTF-8"/>
		<!-- 分页相关 -->
		<plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />
		<!-- 带上序列化接口 -->
		<plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
		<!-- 自定义的注释生成插件-->
		<plugin type="com.chrm.mybatis.generator.plugins.CommentPlugin">
			<!-- 抑制警告 -->
			<property name="suppressTypeWarnings" value="true" />
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="false" />
			<!-- 是否生成注释代时间戳-->
			<property name="suppressDate" value="true" />
		</plugin>
		<!-- 整合lombok-->
		<plugin type="com.chrm.mybatis.generator.plugins.LombokPlugin" >
			<property name="hasLombok" value="true"/>
		</plugin>

		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
						connectionURL="jdbc:mysql://localhost:3306/sf-quiz?useUnicode=true&amp;characterEncoding=UTF-8"
						userId="root" password="362427gg">
		</jdbcConnection>

		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- 实体生成目录配置 -->
		<javaModelGenerator targetPackage="com.chrm.inforsServer.dataobject"
							targetProject="src/main/java">
			<property name="enableSubPackages" value="false" />
			<property name="trimStrings" value="true" />
		</javaModelGenerator>

		<!-- mapper.xml接口生成目录配置 -->
		<sqlMapGenerator targetPackage="sqlmap/com/chrm/inforsServer.mapper" targetProject="src/main/resources">
			<property name="enableSubPackages" value="true" />
		</sqlMapGenerator>

		<!-- mapper接口生成目录配置 -->
		<javaClientGenerator type="XMLMAPPER"
							 targetPackage="com.chrm.inforsServer" targetProject="src/main/java">
			<property name="enableSubPackages" value="true" />
		</javaClientGenerator>

		<!--表格实体配置-->
		<table tableName="award" domainObjectName="AwardDo">
			<generatedKey column="award_id" sqlStatement="JDBC" identity="true" />
		</table>

	</context>
</generatorConfiguration>
```