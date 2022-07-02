
* [一、Mybatis介绍](#一mybatis介绍)
* [二、使用Mybatis](#二使用mybatis)
  * [2\.1 引入Mybatis依赖](#21-引入mybatis依赖)
  * [2\.2 xml配置文件配置数据库连接](#22-xml配置文件配置数据库连接)
  * [2\.3 SqlSessionFactory和SqlSession](#23-sqlsessionfactory和sqlsession)
  * [2\.4 编写实体类](#24-编写实体类)
  * [2\.5 编写Mapper接口类](#25-编写mapper接口类)
  * [2\.6 为Mapper类编写Mapper\.xml配置文件](#26-为mapper类编写mapperxml配置文件)
  * [2\.7 使用session进行数据库操作](#27-使用session进行数据库操作)

# 一、Mybatis介绍

MyBatis 是一款优秀的持久层框架,它支持自定义 SQL、存储过程以及高级映射。

MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程

MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【PlainOld Java Objects,普通的 Java对象】映射成数据库中的记录。

MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。

2013年11月迁移到Github .

Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html

GitHub : https://github.com/mybatis/mybatis-3

# 二、使用Mybatis

## 2.1 引入Mybatis依赖

如果使用Maven来构建项目，需要将下面的依赖置于pom.xml文件中

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>x.x.x</version>
</dependency>
```

## 2.2 xml配置文件配置数据库连接

XML 配置文件中包含了对 MyBatis 系统的核心设置，包括获取数据库连接实例的数据源（DataSource）以及决定事务作用域和控制方式的事务管理器（TransactionManager）。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

## 2.3 SqlSessionFactory和SqlSession

MyBatis的核心是SqlSessionFactory和SqlSession。通过SqlSessionFactoryBuilder创建一个读取了数据库配置的SqlSessionFactory，然后通过SqlSessionFactory创建SqlSession实现对数据的操作。

```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import java.io.IOException;
import java.io.InputStream;

public class MybatisUtils {
private static SqlSessionFactory sqlSessionFactory;
static {
    try {
       String resource = "mybatis-config.xml";
       InputStream inputStream =Resources.getResourceAsStream(resource);
       sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    } catch (IOException e) {
        e.printStackTrace();
    }
}
//获取SqlSession连接
public static SqlSession getSession(){
    return sqlSessionFactory.openSession();
}

}
```

也可以用代码配置数据库连接，不通过xml配置的方式来实现数据库配置。

```java
DataSource dataSource = BlogDataSourceFactory.getBlogDataSource();
TransactionFactory transactionFactory = new JdbcTransactionFactory();
Environment environment = new Environment("development", transactionFactory, dataSource);
Configuration configuration = new Configuration(environment);
configuration.addMapper(BlogMapper.class);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);
```

## 2.4 编写实体类

```java
public class User {
    private int id; //id
    private String name; //姓名
    private String pwd; //密码
    //构造,有参,无参
    //set/get
    //toString()
}
```

## 2.5 编写Mapper接口类

```
import com.kuang.pojo.User;
import java.util.List;
public interface UserMapper {
List<User> selectUser();
}
```

## 2.6 为Mapper类编写Mapper.xml配置文件
```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.kuang.dao.UserMapper">
<select id="selectUser" resultType="com.kuang.pojo.User">
select * from user
</select>
</mapper>
```


## 2.7 使用session进行数据库操作

```java
public class MyTest {
@Test
public void selectUser() {
SqlSession session = MybatisUtils.getSession();
//方法一:
//List<User> users =
session.selectList("com.kuang.mapper.UserMapper.selectUser");
//方法二:
UserMapper mapper = session.getMapper(UserMapper.class);
List<User> users = mapper.selectUser();
for (User user: users){
System.out.println(user);
}
session.close();
}
}
```




