# 一、pagehelper介绍

pageHelper是一款非常简单、易用的分页插件，它能很好的集成在spring boot中。它是一个基于mybatis的一款插件，所以我们在使用它时，我们需要使用mybatis作为持久层框架。

github地址是https://github.com/pagehelper/Mybatis-PageHelper

# 二、整合pagehelper

## pom 配置

增加pageHelper的依赖
```xml
<!-- pagehelper -->
<dependency>    
    <groupId>com.github.pagehelper</groupId>    
    <artifactId>pagehelper-spring-boot-starter</artifactId>
</dependency>
```

注意：spring boot 引入的jar包必须是要pagehelper-spring-boot-starter ，如果单独引入pagehelper的话，会提示错误。

## application.properties增加pagehelper 配置

```
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

说明： 

```
helperDialect ：指定数据库，可以不配置，pagehelper插件会自动检测数据库的类型。

resonable ：分页合理化参数默认false，当该参数设置为true 时，pageNum <= 0 时，默认显示第一页，pageNum 超过 pageSize 时，显示最后一页。

params ：用于从对象中根据属性名取值，可以配置pageNum，pageSize，count 不用配置映射的默认值。

supportMethodsArguments ：分页插件会根据查询方法的参数中，自动根据params 配置的字段中取值，找到合适的值会自动分页。　
```

到这里配置就完成了，在Springboot中整合就是这么简洁，约定大约配置的方式，大量的减少了配置文件的使用 。

## 实现分页

在原来的UserService类和UserServiceImpl 类中，增加 queryUserListPaged 接口和对应的方法实现。

```java
@Override
public List<SysUser> queryUserListPaged(SysUser user, Integer page, Integer pageSize) {
    // 开始分页    
    PageHelper.startPage(page,pageSize);    
    Example example = new Example(SysUser.class);    
    Example.Criteria criteria = example.createCriteria();
    if (!StringUtils.isEmptyOrWhitespace(user.getUsername())) {
            criteria.andLike("username", "%" + user.getUsername() + "%");    
    }
    if (!StringUtils.isEmptyOrWhitespace(user.getNickname())) {
            criteria.andLike("nickname", "%" + user.getNickname() + "%");    
    }
    
    List<SysUser> userList = userMapper.selectByExample(example);
    
    return userList;
}
```

分页的核心就一行代码， PageHelper.startPage(page,pageSize); 这个就表示开始分页。加了这个之后pagehelper 插件就会通过其内部的拦截器，将执行的sql语句，转化为分页的sql语句。

注意：使用时PageHelper.startPage(pageNum, pageSize)一定要放在列表查询的方法中，这样在查询时会查出相应的数据量且会查询出总数。

## 在原来的MybatisController 控制器中增加如下方法：
```java
@RequestMapping("/queryUserListPaged")
public JSONResult queryUserListPaged(Integer page) {
    if (page == null) {
            page = 1;    
    }
    int pageSize = 10;
    SysUser user = new SysUser();
    List<SysUser> userList = userService.queryUserListPaged(user, page, pageSize);
    return JSONResult.ok(userList);
}
```
说明：调用的方法，只需将page页码，和pageSize 每页多少条这两个字段传入即可。

