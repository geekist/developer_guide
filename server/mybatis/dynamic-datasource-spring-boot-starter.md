微服务架构中，分库分表是常有的操作。在多个数据源的情况下，我们如何在代码中灵活切换成为问题。今天我们就利用dynamic-datasource-spring-boot-starter组件来实现多数据源配置

一、介绍

dynamic-datasource-spring-boot-starter 是一个基于springboot的快速集成多数据源的启动器。

其支持 Jdk 1.7+, SpringBoot 1.4.x 1.5.x 2.x.x。

github：https://github.com/baomidou/dynamic-datasource-spring-boot-starter

## 特性

* 支持 数据源分组 ，适用于多种场景 纯粹多库 读写分离 一主多从 混合模式。

* 支持数据库敏感配置信息 加密(可自定义) ENC()。

* 支持每个数据库独立初始化表结构schema和数据库database。

* 支持无数据源启动，支持懒加载数据源（需要的时候再创建连接）。

* 支持 自定义注解 ，需继承DS(3.2.0+)。

* 提供并简化对Druid，HikariCp，BeeCp,Dbcp2的快速集成。

* 提供对Mybatis-Plus，Quartz，ShardingJdbc，P6sy，Jndi等组件的集成方案。

* 提供 自定义数据源来源 方案（如全从数据库加载）。

* 提供项目启动后 动态增加移除数据源 方案。

* 提供Mybatis环境下的 纯读写分离 方案。

* 提供使用 spel动态参数 解析数据源方案。内置spel，session，header，支持自定义。

* 支持 多层数据源嵌套切换 。（ServiceA >>> ServiceB >>> ServiceC）。

* 提供 基于seata的分布式事务方案 。

* 提供 本地多数据源事务方案。

## 约定

本框架只做 切换数据源 这件核心的事情，并不限制你的具体操作，切换了数据源可以做任何CRUD。

配置文件所有以下划线 _ 分割的数据源 首部 即为组的名称，相同组名称的数据源会放在一个组下。

切换数据源可以是组名，也可以是具体数据源名称。组名则切换时采用负载均衡算法切换。

默认的数据源名称为 master ，你可以通过 spring.datasource.dynamic.primary 修改。

方法上的注解优先于类上注解。

DS支持继承抽象类上的DS，暂不支持继承接口上的DS。

# 二、 使用


## 引入dynamic-datasource-spring-boot-starter
```xml
<dependency>
  <groupId>com.baomidou</groupId>
  <artifactId>dynamic-datasource-spring-boot-starter</artifactId>
  <version>${version}</version>
</dependency>
```

## 配置数据源

```yml
# 数据源配置
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

```      
# 多主多从                      纯粹多库（记得设置primary）                   混合配置
spring:                               spring:                               spring:
  datasource:                           datasource:                           datasource:
    dynamic:                              dynamic:                              dynamic:
      datasource:                           datasource:                           datasource:
        master_1:                             mysql:                                master:
        master_2:                             oracle:                               slave_1:
        slave_1:                              sqlserver:                            slave_2:
        slave_2:                              postgresql:                           oracle_1:
        slave_3:                              h2:                                   oracle_2:
```

## 使用 @DS 切换数据源

@DS 可以注解在方法上或类上，同时存在就近原则 方法上注解 优先于 类上注解。

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
