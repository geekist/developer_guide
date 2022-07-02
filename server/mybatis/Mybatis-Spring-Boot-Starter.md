* [一、MyBatis\-Spring\-Boot\-Starter 简介](#一mybatis-spring-boot-starter-简介)
* [二、 MyBatis\-Spring\-Boot\-Starter 使用](#二-mybatis-spring-boot-starter-使用)
  * [2\.1 引入mybatis\-spring\-boot\-starter依赖](#21-引入mybatis-spring-boot-starter依赖)
  * [2\.2在application\.yml中配置数据源](#22在applicationyml中配置数据源)
  * [2\.3 编写Mapper接口类](#23-编写mapper接口类)
  * [2\.4 编写Mapper配置文件](#24-编写mapper配置文件)
  * [2\.5 在启动类中添加对 mapper 包扫描@MapperScan，自动注入所有的mapper](#25-在启动类中添加对-mapper-包扫描mapperscan 自动注入所有的mapper)
  * [2\.6 使用Mapper进行数据库操作](#26-使用mapper进行数据库操作)


# 一、MyBatis-Spring-Boot-Starter 简介

MyBatis-Spring-Boot-Starter 是 mybatis 为 springboot 提供的快速集成的方案（因为 springboot 太火了），原话是 The MyBatis-Spring-Boot-Starter help you build quickly MyBatis applications on top of the Spring Boot。因此如果项目中使用 springboot 和 mybatis 的话，这个 starter 可以大大的简化你的工作。

MyBatis-Spring-Boot-Starter类似一个中间件，链接Spring Boot和MyBatis，构建基于Spring Boot的MyBatis应用程序。

MyBatis-Spring-Boot-Starter 当前版本是 2.1.2，发布于2020年3月10日

MyBatis-Spring-Boot-Starter是个集成包，因此对MyBatis、MyBatis-Spring和SpringBoot的jar包都存在依赖，如下所示：

| MyBatis-Spring-Boot-Starter |	MyBatis-Spring | Spring Boot	| Java |
| ----  | ----  | ---- | ---- |
| 2.1	| 2.0 (need 2.0.2+ for enable all features)	| 2.1 or higher | 	8 or higher |
| 1.3	| 1.3 |	1.5	| 6 or higher |


# 二、 MyBatis-Spring-Boot-Starter 使用

众所周知，MyBatis的核心有两大组件：SqlSessionFactory 和 Mapper 接口。前者表示数据库链接，后者表示SQL映射。当我们基于Spring使用MyBatis的时候，也要保证在Spring环境中能存在着两大组件。

MyBatis-Spring-Boot-Starter功能

* 1、Autodetect an existing DataSource

自动发现存在的DataSource

* 2、Will create and register an instance of a SqlSessionFactory passing that DataSource as an input using the SqlSessionFactoryBean

利用SqlSessionFactoryBean创建并注册SqlSessionFactory

* 3、Will create and register an instance of a SqlSessionTemplate got out of the SqlSessionFactory

创建并注册SqlSessionTemplate

* 4、Auto-scan your mappers, link them to the SqlSessionTemplate and register them to Spring context so they can be injected into your beans

自动扫描Mappers，并注册到Spring上下文环境方便程序的注入使用

Spring Boot 会自动加载 spring.datasource.* 相关配置，数据源就会自动注入到 sqlSessionFactory 中，sqlSessionFactory 会自动注入到 Mapper 中，对了，你一切都不用管了，直接拿起来使用就行了。

## 2.1 引入mybatis-spring-boot-starter依赖

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>
```

## 2.2在application.yml中配置数据源

mybatis-spring-boot-starter会自动读取数据源中的配置。无需代码实现。

springboot中，使用苞米豆开发的多数据源工具dynamic-datasource-spring-boot-starter来进行多数据库配置。

```yml
spring:
  datasource:
    dynamic:
      primary: master #设置默认的数据源或者数据源组,默认值即为master
      datasource:
        master:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_biz?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        user:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_user?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        class:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_class?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        task:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_task?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
        message:
          url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_message?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
          username: employ
          password: employ_2020
          driver-class-name: com.mysql.cj.jdbc.Driver
```



## 2.3 编写Mapper接口类

可以采用tkmybatis工具简化xml的编写。

```java

package com.yuya.console.mapper.msg;

import com.yuya.common.entity.console.msg.MBaby;

import tk.mybatis.mapper.common.Mapper;

/**   
 * <p>自动生成工具：mybatis-dsc-generator</p>
 * 
 * <p>说明： 数据访问层</p>
 * @version: V1.0
 * @author: 杭州育伢
 * 
 */
public interface MBabyMapper extends Mapper<MBaby>{
	public int deleteByIds(String[] ids);
}
```

## 2.4 编写Mapper配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yuya.console.mapper.msg.MBabyMapper">
  <resultMap id="BaseResultMap" type="com.yuya.common.entity.console.msg.MBaby">
    <!--
      WARNING - @mbg.generated
    -->
    <id column="id" jdbcType="INTEGER" property="id" />
    <result column="ins_id" jdbcType="INTEGER" property="insId" />
    <result column="head_url" jdbcType="VARCHAR" property="headUrl" />
    <result column="baby_real_name" jdbcType="VARCHAR" property="babyRealName" />
    <result column="baby_nick_name" jdbcType="VARCHAR" property="babyNickName" />
    <result column="campus_id" jdbcType="INTEGER" property="campusId" />
    <result column="clazz_id" jdbcType="INTEGER" property="clazzId" />
  </resultMap>
  <sql id="Base_Column_List">
    <!--
      WARNING - @mbg.generated
    -->
    id, ins_id, head_url, baby_real_name, baby_nick_name, campus_id, clazz_id
  </sql>

    <delete id="deleteByIds" parameterType="String">
        delete from m_baby where id in 
        <foreach close=")" collection="array" item="id" open="(" separator=",">
            #{id}
        </foreach>
    </delete>
</mapper>
```

## 2.5 在启动类中添加对 mapper 包扫描@MapperScan，自动注入所有的mapper

```java
@EnableWebMvc
//@ImportResource(locations = "classpath:redis/*.xml")
@ComponentScan(basePackages = {"com.yuya.console", "com.yuya.common"})
@MapperScan({"com.yuya.console.mapper"})
@ConditionalOnClass(SpringfoxWebMvcConfiguration.class)
@SpringBootApplication(exclude = DruidDataSourceAutoConfigure.class)
public class YychildrenConsoleApplication {

	public static void main(String[] args) {
		SpringApplication.run(YychildrenConsoleApplication.class, args);
	}
}
```
或者直接在 Mapper 类上面添加注解@Mapper，建议使用上面那种，不然每个 mapper 加个注解也挺麻烦

## 2.6 使用Mapper进行数据库操作

```java

package com.yuya.console.service.msg.impl;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.baomidou.dynamic.datasource.annotation.DS;
import com.yuya.common.entity.console.msg.MBaby;
import com.yuya.console.mapper.msg.MBabyMapper;
import com.yuya.console.service.msg.MBabyService;

import cn.hutool.core.convert.Convert;

@Service
@DS("message")
public class MBabyServiceImpl implements MBabyService {

	@Autowired
	MBabyMapper mBabyMapper;

	@Override
	public int deleteByIds(String ids) {
		return mBabyMapper.deleteByIds(Convert.toStrArray(ids));
	}

	@Override
	public int insert(MBaby mBaby) {
		return mBabyMapper.insert(mBaby);
	}

	@Override
	public int update(MBaby map) {
		return mBabyMapper.updateByPrimaryKeySelective(map);
	}
}
```