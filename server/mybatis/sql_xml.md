
* [一、mybatis的mapper映射文件概述](#一mybatis的mapper映射文件概述)
* [二、mapper文件的结构](#二mapper文件的结构)
* [三、xml元素详解](#三xml元素详解)
  * [resultMap](#resultmap)
  * [select](#select)
    * [返回单值](#返回单值)
    * [返回单对象](#返回单对象)
      * [通过javabean返回单对象](#通过javabean返回单对象)
      * [通过map返回单对象](#通过map返回单对象)
    * [返回数据集](#返回数据集)
      * [通过list返回数据集](#通过list返回数据集)
      * [通过map返回数据集](#通过map返回数据集)
  * [insert](#insert)
  * [update](#update)
  * [delete](#delete)
* [四、动态SQL](#四动态sql)
  * [if语句](#if语句)
  * [if \+ where 语句](#if--where-语句)
  * [if \+ set 语句](#if--set-语句)
  * [choose(when,otherwise) 语句](#choosewhenotherwise-语句)
  * [trim语句](#trim语句)
  * [SQL片段](#sql片段)
  * [foreach语句](#foreach语句)


# 一、mybatis的mapper映射文件概述

MyBatis配置文件分为：mybatis配置文件和mapper配置文件。mybatis配置文件主要用来设计po类的别名，方便对po类的引用；mapper配置文件用来管理SQL语句。

在mybatis开发中，涉及到主要开发要素是:dao接口类，mapper映射文件，以及po类。它们之间的关系如下：

![](./assets/mybatis_1.png)

dao接口类中，定义了数据库操作的接口方法，主要包含增，删，改，查等接口方法；po类定义接口方法的参数，可使用po类保存查询结果，或者为insert，update方法提供数据集参数。操作数据库表的SQL语句保存在mapper映射文件中。mapper映射文件分别提供select,insert,update,delete xml元素，分别对应数据库的查询，插入，修改，删除操作。每一个xml元素通过id属性与dao接口类中的方法相互关联。

# 二、mapper文件的结构

mapper映射文件是xml格式的配置文件，由一系列具有层级关系的元素组成。并且通过元素的属性，这些元素之间具有关联关系。具体情况如下图所示：

![](./assets/mybatis_2.png)


在mapper映射文件中，主要包含如下配置元素：

* mapper元素

该元素是最顶层的配置元素，其属性namespace指向IDao类的全类型名，即：包路径+类名。

在mapper元素下面，包含如下子元素：resultMap元素，select元素，insert元素，update元素，delete元素。

* resultMap元素

建立数据库表的列名与po类的数据字段之间的映射关系。当po类的数据字段与数据库表不一致的时候，或者承载复杂查询结果的时候，使用resultMap配置；

* select元素

用来维护select语句。

* insert元素

用来维护insert语句。

* update元素

用来维护update语句。

* delete元素

用来维护delete语句。

常见的xml文件结构如下所示：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yuya.core.mapper.CCourseCollectionMapper">
  <resultMap id="BaseResultMap" type="com.yuya.common.entity.console.organ.CCourseCollection">
    <!--
      WARNING - @mbg.generated
    -->
    <id column="id" jdbcType="INTEGER" property="id" />
    <result column="title" jdbcType="VARCHAR" property="title" />
    <result column="cover_url" jdbcType="VARCHAR" property="coverUrl" />
    <result column="description" jdbcType="VARCHAR" property="description" />
    <result column="lecturer" jdbcType="VARCHAR" property="lecturer" />
    <result column="total_num" jdbcType="INTEGER" property="totalNum" />
    <result column="update_num" jdbcType="INTEGER" property="updateNum" />
    <result column="is_top" jdbcType="BIT" property="isTop" />
    <result column="create_time" jdbcType="TIMESTAMP" property="createTime" />
    <result column="update_time" jdbcType="TIMESTAMP" property="updateTime" />
    <result column="type" jdbcType="INTEGER" property="type" />
    <result column="is_deleted" jdbcType="BIT" property="isDeleted" />    
    <result column="content" jdbcType="LONGVARCHAR" property="content" />
  </resultMap>
  
  <sql id="Base_Column_List">
    id, title, cover_url, description, lecturer, total_num, update_num, is_top, create_time, 
    update_time, type, is_deleted,content
  </sql>
  
  <select id="findById" resultMap="BaseResultMap">
  	select <include refid="Base_Column_List"/> from c_course_collection where is_deleted = 0 and id = #{id}
  </select>
  ......
</mapper>
```
# 三、xml元素详解

## resultMap

使用resultMap配置节，建立po类的数据字段与数据库表的列名之间的映射关系。当po类的数据字段与数据库表的列名不一致的时候，可使用该配置。 resultMap的代码示例如下：
```xml
<resultMap type="UserInfo" id="userData">
        <id property="id" column="f_id" />
        <result property="name" column="f_name" />
        <result property="birth" column="f_birth" />
        <result property="salary" column="f_salary" />
</resultMap>
```

type属性执行po类的全类型名，即：包名+类名。如果在mybatis配置文件中，为po类建立了别名，那么type属性可以引用该别名。

id属性应该全局唯一，它被select元素的resultMap属性引用。

\<id>子元素用来建立po数据字段与数据库表主键列之间的映射关系。

\<result>子元素用来建立po数据字段与数据库表非主键列之间的映射关系。

property属性用来指定po类的数据字段名。例如：private String name；该定义中的name。

column属性用来指定数据库表的列名。

```xml
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
```

## select

在mapper文件中，使用select元素来管理select语句。


### 返回单值

当查询结果返回一个单值的时候，其配置如下：
```xml
<select id="getUserCount" resultType="int">
  select count(*) from t_user
</select>
```
使用resultType属性指定返回结果的数据类型，这里可以是java基本数据类型，如：int,String等。

在IDao类中，接口方法定义如下：
```java
int getUserCount();
//方法的返回值类型是int，与resultType的值对应；方法的名称是getUserCount与id属性的值对应。
```
### 返回单对象

单对象的含义是：查询结果包含多个数据列，但只有一行数据。可以使用javabean或者map来保存查询结果。

#### 通过javabean返回单对象

通过javabean返回查询结果的配置如下：
```xml
<select id="getUserInfoById" resultMap="userData">
select * from t_user
where f_id = #{id}
</select>
```
当po类的数据字段与数据库表的列名完全一致的时候，可使用resultType=po类全类型名，或po类别名 的方式指定查询结果的类型。

当po类的数据字段与数据库表的列名不一致的时候，需要定义resultMap映射。在这种情况下，使用resultMap属性，该属性的值与<resultMap>元素的id属性的值相同。

resultMap与resultType不能同时使用。
    
在IDao类中，接口方法的定义如下：

```java
UserInfo getUserInfoById(int id)

//返回值的类型是UserInfo，通过<resultMap>元素的配置，将该类型映射到userData。在<select>元素中，通过resultMap属性引用userData.
```

#### 通过map返回单对象

当查询结果是一个单对象，但是没有定义po类的时候，可使用map来承载查询结果。map的key是数据库表的列名，value是该列对应的值。mapper映射文件的配置如下：
```xml
<select id="getUserInfoToMap" resultType="hashmap">
select * from t_user where f_id=#{id}
</select>
```
resultType属性的值设定为hashmap，或者map即可。IDao类中，接口方法定义如下：

```java
Map<String,Object> getUserInfoToMap(int id);
```

### 返回数据集

数据集的含义是：包含多行数据，每行数据包含多列。

#### 通过list返回数据集

当查询结果返回一个数据集的时候，mybatis会将查询结果放入list中，然后返回。在这种情况下,resultTpye或者resultMap属性指定的是list列表元素的类型，而非集合本身。mapper映射文件的配置如下：
```xml
<select id="listUserInfo" resultMap="userData">
select * from t_user
</select>
```
resultMap或者resultType可以设定为java bean ，也可设定为map。该配置形式与返回单对象相同，其差别在IDao类中的接口方法定义：

```java
list<UserInfo> listUserInfo();
//在定义接口方法的时候，需要使用list包装单值对象的类型。
```
 
#### 通过map返回数据集

该方式需要与注解@MapKey配合使用。map的key是：数据库表中的任意一列，一般选具有索引性质的列，如：id，name等。value是：po类的引用，或者另外一个map的引用。mapper配置文件的内容如下：
```
<select id="listUserInfoToMap" resultType="map">
select * from t_user
</select>
```

在IDao类中，接口方法的定义如下：

```java
@MapKey("f_id")
Map<BigInteger,Map<String,Object>> listUserInfoToMap();
```

在上面的示例中，value的值是map类型。value的值也可以是po类。在使用po类的时候，mapper映射文件的配置如下：

```xml
<select id="listUserInfo" resultMap="userData">
select * from t_user
</select>
```
在IDao类中，接口方法定义如下：

```java
@MapKey("id")
map<int,UserInfo> listUserInfo();
```

注：
resultType属性的值可以是如下情形：
>1.基本数据类型，如：int,String等；
>2.class数据类型，如：java bean，这里输入的是全类型名或者别名;
>3.map数据类型。包括：单对象和集合两种；
>4.集合数据类型，是集合元素的类型，而非集合本身。

resultMap的使用：
>该属性的值是：<resultMap>元素的id属性的值。只有当po数据字段与数据库表列名不一致的时候，才使用。

## insert

在mybatis中，使用<insert>元素来管理insert语句。如果sql语句比较简单，可使用@Insert注解来代替mapper映射文件。

如果需要插入的数据字段比较多，可以使用po类封装这些数据字段，示例如下：

```xml
<insert id="insertUserInfoByBean" parameterType="UserInfo">
insert into t_user (f_name,f_birth,f_salary) values (#{name},#{birth},#{salary})
</insert>
```
IDao类的接口方法定义如下：
```java
int insertUserInfoByBean(UserInfo userInfo);
```
<insert>元素的id属性值是IDao类接口方法的名称。

<insert>元素的parameterType属性值可以省略。其值是接口方法参数的类型。如果接口方法的参数是java bean，那么该值是全类型名（即：包名+类名），或者是类型的别名。

注意：在sql语句中，如：#{name}，#{birth}等参数，其名称：name,birth，必须与接口参数UserInfo的数据字段一致。即：UserInfo的数据字段名称必须是name,birth。否则无法识别。

如果需要插叙的数据字段比较少，那么可以直接传递给参数到接口方法中，mapper映射文件的示例如下：

```xml
<insert id="insertUserInfo" >
insert into t_user (f_name,f_birth,f_salary) values (#{name},#{birth},#{salary})
</insert>
```

IDao接口类的定义如下：

```java
int insertUserInfo(@Param("name") String userName,@Param("birth") Date birthDay,@Param("salary") double salary);
```

注解@Param用来指定接口方法的参数名与sql语句中的参数名之间的映射。@Param注解的参数，如：name，必须与sql语句中的参数，如：#{name}中的name 是完全一致的。

如果sql语句比较简单，可以使用注解来代替mapper映射文件，示例如下：

```java
@Insert("{insert into t_user (f_name,f_birth,f_salary) values (#{name},#{birth},#{salary})}")
int insertUserInfo(@Param("name") String userName,@Param("birth") Date birthDay,@Param("salary") double salary);
```

## update

<update>元素用来维护update类型的sql语句。如果该sql语句比较简单，可以使用@Update注解代替mapper映射文件。


## delete 

<delete>元素用来维护delete类型的sql语句。如果该sql语句比较简单，可以使用@Delete注解来代替mapper映射文件。

# 四、动态SQL

 使用动态sql来支持复杂的sql语句。在动态sql部分，包含如下xml元素：if, choose, when, otherwise, trim, where, set, foreach。

假设一个数据库表的元素定义如下：
```sql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `username` VARCHAR(50)   NULL,
   `sex` VARCHAR(10) NULL,
   `birthday` DATE NULL,
   `address` DATE NULL,
   PRIMARY KEY ( `id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8md4;
```

## if语句

根据 username 和 sex 来查询数据。如果username为空，那么将只根据sex来查询；反之只根据username来查询。首先不使用 动态SQL 来书写

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
<!-- 这里和普通的sql 查询语句差不多，对于只有一个参数，后面的 #{id}表示占位符，里面不一定要写id,
写啥都可以，但是不要空着，如果有多个参数则必须写pojo类里面的属性 -->
select * from user where username=#{username} and sex=#{sex}
</select>
```
    
上面的查询语句，我们可以发现，如果 #{username} 为空，那么查询结果也是空，如何解决这个问题呢？使用if来判断

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
select * from user where
<if test="username != null">
   username=#{username}
</if>

<if test="username != null">
and sex=#{sex}
</if>
</select>
```

## if + where 语句

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
select * from user

<where>

<if test="username != null">
username=#{username}
</if>

<if test="username != null">
and sex=#{sex}
</if>

</where>
</select>
```
这个“where”标签会知道如果它包含的标签中有返回值的话，它就插入一个‘where’。此外，如果标签返回的内容是以AND 或OR 开头的，则它会剔除掉。

## if + set 语句

同理，上面的对于查询 SQL 语句包含 where 关键字，如果在进行更新操作的时候，含有 set 关键词，我们怎么处理呢？

```xml
<!-- 根据 id 更新 user 表的数据 -->
<update id="updateUserById" parameterType="com.ys.po.User">
update user u
<set>
<if test="username != null and username != ''">
u.username = #{username},
</if>
<if test="sex != null and sex != ''">
u.sex = #{sex}
</if>
</set>

where id=#{id}
</update>
```
 

这样写，如果第一个条件 username 为空，那么 sql 语句为：update user u set u.sex=? where id=? ； 如果第一个条件不为空，那么 sql 语句为：update user u set u.username = ? ,u.sex = ? where id=?

## choose(when,otherwise) 语句

有时候，我们不想用到所有的查询条件，只想选择其中的一个，查询条件有一个满足即可，使用 choose 标签可以解决此类问题，类似于 Java 的 switch 语句

```xml
<select id="selectUserByChoose" resultType="com.ys.po.User" parameterType="com.ys.po.User">
select * from user
<where>
<choose>
<when test="id !='' and id != null">
id=#{id}
</when>
<when test="username !='' and username != null">
and username=#{username}
</when>
<otherwise>
and sex=#{sex}
</otherwise>
</choose>
</where>
</select>
```
也就是说，这里我们有三个条件，id,username,sex，只能选择一个作为查询条件

如果 id 不为空，那么查询语句为：select * from user where id=?

如果 id 为空，那么看username 是否为空，如果不为空，那么语句为 select * from user where username=?;

如果 username 为空，那么查询语句为 select * from user where sex=?

## trim语句

trim标记是一个格式化的标记，可以完成set或者是where标记的功能

用 trim 改写上面第二点的 if+where 语句

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
select * from user
<!-- <where>
<if test="username != null">
username=#{username}
</if>

<if test="username != null">
and sex=#{sex}
</if>
</where> -->
<trim prefix="where" prefixOverrides="and | or">
<if test="username != null">
and username=#{username}
</if>
<if test="sex != null">
and sex=#{sex}
</if>
</trim>
</select>
```
 
prefix：前缀；prefixoverride：去掉第一个and或者是or。

用 trim 改写上面第三点的 if+set 语句

```xml
<!-- 根据 id 更新 user 表的数据 -->
<update id="updateUserById" parameterType="com.ys.po.User">
update user u
<!-- <set>
<if test="username != null and username != ''">
u.username = #{username},
</if>
<if test="sex != null and sex != ''">
u.sex = #{sex}
</if>
</set> -->
<trim prefix="set" suffixOverrides=",">
<if test="username != null and username != ''">
u.username = #{username},
</if>
<if test="sex != null and sex != ''">
u.sex = #{sex},
</if>
</trim>

where id=#{id}
</update>　　 　
```
 suffix：后缀　　
suffixoverride：去掉最后一个逗号（也可以是其他的标记，就像是上面前缀中的and一样）　


## SQL片段

有时候可能某个 sql 语句我们用的特别多，为了增加代码的重用性，简化代码，我们需要将这些代码抽取出来，然后使用时直接调用。

比如：假如我们需要经常根据用户名和性别来进行联合查询，那么我们就把这个代码抽取出来，如下：

```xml
<!-- 定义 sql 片段 -->
<sql id="selectUserByUserNameAndSexSQL">
<if test="username != null and username != ''">
AND username = #{username}
</if>
<if test="sex != null and sex != ''">
AND sex = #{sex}
</if>
</sql>
```
 

引用 sql 片段

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
select * from user
<trim prefix="where" prefixOverrides="and | or">
<!-- 引用 sql 片段，如果refid 指定的不在本文件中，那么需要在前面加上 namespace -->
<include refid="selectUserByUserNameAndSexSQL"></include>
<!-- 在这里还可以引用其他的 sql 片段 -->
</trim>
</select>
```
 

注意：
>最好基于 单表来定义 sql 片段，提高片段的可重用性
>
>在 sql 片段中不要包括 where

## foreach语句

需求：我们需要查询 user 表中 id 分别为1,2,3的用户

sql语句：select * from user where id=1 or id=2 or id=3
　　　　　select * from user where id in (1,2,3)

建立一个 UserVo 类，里面封装一个 List<Integer> ids 的属性

```java
package com.ys.vo;

import java.util.List;

public class UserVo {
//封装多个用户的id
private List<Integer> ids;

public List<Integer> getIds() {
return ids;
}

public void setIds(List<Integer> ids) {
this.ids = ids;
}

}　

```　
 
我们用 foreach 来改写 select * from user where id=1 or id=2 or id=3

```xml
<select id="selectUserByListId" parameterType="com.ys.vo.UserVo" resultType="com.ys.po.User">
select * from user
<where>
<!--
collection:指定输入对象中的集合属性。该属性的值有三种：list,array,map，根据传入的集合类型而设定该值。
item:每次遍历生成的对象
index:当前迭代的次数
open:开始遍历时的拼接字符串
close:结束时拼接的字符串
separator:遍历对象之间需要拼接的字符串
select * from user where 1=1 and (id=1 or id=2 or id=3)
-->
<foreach collection="list" item="id" open="and (" close=")" separator="or">
id=#{id}
</foreach>
</where>
</select>
```
 

我们用 foreach 来改写 select * from user where id in (1,2,3)

```xml
<select id="selectUserByListId" parameterType="com.ys.vo.UserVo" resultType="com.ys.po.User">
select * from user
<where>
<!--
collection:指定输入对象中的集合属性.该属性的值有三种：list,array,map，根据传入的集合类型而设定该值。
item:每次遍历生成的对象
index:当前迭代的次数
open:开始遍历时的拼接字符串
close:结束时拼接的字符串
separator:遍历对象之间需要拼接的字符串
select * from user where 1=1 and id in (1,2,3)
-->
<foreach collection="list" item="id" open="and id in (" close=") " separator=",">
#{id}
</foreach>
</where>
</select>
```