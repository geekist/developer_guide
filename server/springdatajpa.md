## Spring Data JPA介绍

　　可以理解为JPA规范的再次封装抽象，底层还是使用了Hibernate的JPA技术实现，引用JPQL（Java Persistence Query Language）查询语言，属于Spring整个生态体系的一部分。随着Spring Boot和Spring Cloud在市场上的流行，Spring Data JPA也逐渐进入大家的视野，它们组成有机的整体，使用起来比较方便，加快了开发的效率，使开发者不需要关心和配置更多的东西，完全可以沉浸在Spring的完整生态标准实现下。JPA上手简单，开发效率高，对对象的支持比较好，又有很大的灵活性，市场的认可度越来越高。

## Step1:创建一个Spring项目

在idea中新建一个spring initializer的工程，选择spring web依赖。

## Step2：在工程中添加数据库的依赖项：

```xml

         <!-- jpa -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
            <version>2.6.0</version>
        </dependency>

        <!-- Mysql驱动包 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!--阿里数据库连接池 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.14</version>
        </dependency>

```


## Step3: 添加数据库访问的配置

在application的yml文件中，添加对数据库访问的配置

```xml

spring:
  application:
    name:
  profiles:
    active: dev

  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:mysql://rm-bp1jzyd0u8yict1n1eo.mysql.rds.aliyuncs.com:3306/test_qlm_biz?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8&allowMultiQueries=true
    username: employ
    password: employ_2020

```

## Step4：用代码访问远程数据库的某一张表

### 4.1 数据库表对应的类

```java

import javax.persistence.*;
import java.util.Date;


@Entity(name="sys_role")
public class SysRole {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int role_id;

    @Column(name="role_name",nullable = true)
    private String role_name;

    @Column
    private String role_key;

    @Column
    private int role_sort;

    @Column
    private int data_scope;

    @Column
    private int status;

    @Column
    private int del_flag;

    @Column
    private String create_by;

    @Column
    private Date create_time;

    @Column
    private String update_by;

    @Column
    private Date update_time;

    @Column
    private String remark;

```

### 4.2 数据库访问操作

```java

package com.ytech.jpa;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface SysRoleDao extends JpaRepository<SysRole,Integer> {

}

```

### 4.3 业务逻辑


```java

package com.ytech.jpa;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class SysRoleService {
    @Autowired
    SysRoleDao sysRoleDao;

    public List<SysRole> getAllSysRoles(){
        return sysRoleDao.findAll();
    }
}



```

### 4.4 api接口

```java

package com.ytech.jpa;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

@RestController
public class SysRoleController {

    @Autowired
    SysRoleService sysRoleService;

    @GetMapping("/sysRole")
    public List<SysRole> getSysRoles() {
        return sysRoleService.getAllSysRoles();
    }

    @GetMapping("/")
    public String hello() {
        return "hello,world,i am jdbctemplate";
    }
}

```

