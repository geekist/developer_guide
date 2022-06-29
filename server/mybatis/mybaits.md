

1.1、什么是MyBatis
MyBatis 是一款优秀的持久层框架,它支持自定义 SQL、存储过程以及高级映射。

MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集的过程

MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 实体类 【PlainOld Java Objects,普通的 Java对象】映射成数据库中的记录。

MyBatis 本是apache的一个开源项目ibatis, 2010年这个项目由apache 迁移到了google code，并且改名为MyBatis 。

2013年11月迁移到Github .

Mybatis官方文档 : http://www.mybatis.org/mybatis-3/zh/index.html

GitHub : https://github.com/mybatis/mybatis-3

为什么需要Mybatis

Mybatis就是帮助程序猿将数据存入数据库中 , 和从数据库中取数据 
.
传统的jdbc操作 , 有很多重复代码块 .比如 : 数据取出时的封装 , 数据库的建立连接等等... , 通过框架可以减少重复代码,提高开发效率 .

MyBatis 是一个半自动化的ORM框架 (Object Relationship Mapping) -->对象关系映射所有的事情，不用Mybatis依旧可以做到，只是用了它，所有实现会更加简单！技术没有高低之
分，只有使用这个技术的人有高低之别

MyBatis的优点

简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件就可以了，易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。

灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。 sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。

解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。

提供xml标签，支持编写动态sql。


