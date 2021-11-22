

## Step1:创建一个Spring项目

在idea中新建一个spring initializer的工程，选择spring web依赖。

## Step2：在工程中添加数据库的依赖项：

```xml

  <!-- jdbcTemplate -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
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
            <version>${druid.version}</version>
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

package com.ytech.jdbctemplate;

import java.util.Date;

public class SysRole {

    private int role_id;
    private String role_name;
    private String role_key;
    private int role_sort;
    private int data_scope;
    private int status;
    private int del_flag;
    private String create_by;
    private Date create_time;
    private String update_by;
    private Date update_time;
    private String remark;
}


```

### 4.2 数据库访问操作

```java

package com.ytech.jdbctemplate;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class SysRoleDao {
    @Autowired
    JdbcTemplate jdbcTemplate;

    public List<SysRole> getAllSysRoles() {
        return jdbcTemplate.query("select * from sys_role",new BeanPropertyRowMapper<>(SysRole.class));
    }
}


```

### 4.3 业务逻辑


```java


package com.ytech.jdbctemplate;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class SysRoleService {

    @Autowired
    SysRoleDao sysRoleDao;

    public List<SysRole> getAllSysRoles(){
        return sysRoleDao.getAllSysRoles();
    }

}

```

### 4.4 api接口

```java

package com.ytech.jdbctemplate;

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





