# 一、数据库基本概念


## 1、术语

* 数据库
  
数据库是一个以某种有组织的方式存储的数据集合。

数据库（database） 保存有组织的数据的容器（通常是一个文件或一组文件）。

理解数据库的一种最简单的办法是将其想象为一个文件柜。此文件柜是一个存放数据的物理位置，不管数据是什么以及如何组织的。

* 表

表（table） 某种特定类型数据的结构化清单。

表是一种结构化的文件，可用来存储某种特定类型的数据。表可以保存顾客清单、产品目录，或者其他信息清单。

在你将资料放入自己的文件柜时，并不是随便将它们扔进某个抽屉就完事了，而是在文件柜中创建文件，然后将相关的资料放入特定的文件中。在数据库领域中，这种文件称为表。

* 列

列（column） 表中的一个字段。所有表都是由一个或多个列组成的。

表由列组成。列中存储着表中某部分的信息。

理解列的最好办法是将数据库表想象为一个网格。网格中每一列存储着一条特定的信息。例如，在顾客表中，一个列存储着顾客编号，另一个列存储着顾客名，而地址、城市、州以及邮政编码全都存储在各自的列中。

* 行

行（row） 表中的一个记录。

表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

如果将表想象为网格，网格中垂直的列为表列，水平行为表行。例如，顾客表可以每行存储一个顾客。表中的行数为记录的总数。

* 主键

主键（primary key）①一一列（或一组列），其值能够唯一区分表中每个行。

表中每一行都应该有可以唯一标识自己的一列（或一组列）。一个顾客表可以使用顾客编号列，而订单表可以使用订单ID，雇员表可以使用雇员ID或雇员社会保险号。

表中的任何列都可以作为主键，只要它满足以下条件：
 任意两行都不具有相同的主键值；
 每个行都必须具有一个主键值（主键列不允许NULL值）。

* 数据类型

数据类型（datatype） 所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据。

数据库中每个列都有相应的数据类型。数据类型定义列可以存储的数据种类。例如，如果列中存储的为数字（或许是订单中的物品数），则相应的数据类型应该为数值类型。如果列中存储的是日期、文本、注释、
金额等，则应该用恰当的数据类型规定出来。

数据类型限制可存储在列中的数据种类（例如，防止在数值字段中录入字符值）。数据类型还帮助正确地排序数据，并在优化磁盘使用方面起重要的作用。因此，在创建表时必须对数据类型给予特别的关注。

* SQL

SQL（发音为字母S-Q-L或sequel）是结构化查询语言（Structured QueryLanguage）的缩写。SQL是一种专门用来与数据库通信的语言。

与其他语言（如，英语以及Java和Visual Basic这样的程序设计语言）不一样，SQL由很少的词构成，这是有意而为的。设计SQL的目的是很好地完成一项任务，即提供一种从数据库中读写数据的简单有效的方法。

SQL有如下的优点：
 SQL不是某个特定数据库供应商专有的语言。几乎所有重要的DBMS都支持SQL，所以，学习此语言使你几乎能与所有数据库打交道。

 SQL简单易学。它的语句全都是由描述性很强的英语单词组成，而且这些单词的数目不多。

 SQL尽管看上去很简单，但它实际上是一种强有力的语言，灵活使用其语言元素，可以进行非常复杂和高级的数据库操作。

## 2、MySQL介绍

MySQL是一种DBMS，即它是一种数据库软件。

MySQL已经存在很久了，它在世界范围内得到了广泛的安装和使用。为什么有那么多的公司和开发人员使用MySQL？以下列出其原因。

 成本——MySQL是开放源代码的，一般可以免费使用（甚至可免费修改）。

 性能——MySQL执行很快（非常快）。

 可信赖——某些非常重要和声望很高的公司、站点使用MySQL，这些公司和站点都用MySQL来处理自己的重要数据。

 简单——MySQL很容易安装和使用。

事实上，MySQL受到的唯一真正的批评是它并不总是支持其他DBMS提供的功能和特性。然而，这一点也正在逐步得到改善，MySQL的各个新版本正不断增加新特性、新功能。

# 二、MySQL操作

## 连接数据库

为了连接到MySQL，需要以下信息：
 主机名（计算机名）——如果连接到本地MySQL服务器，为localhost；
 端口（如果使用默认端口3306之外的端口）；
 一个合法的用户名；
 用户口令（如果需要）。

## USE 打开数据库 

```sql
use database_name;
```

## SHOW 查看数据库和表

```sql

show databases;

show tables;

show columns from tb_xxx;

SHOW STATUS;

SHOW CREATE DATABASE;

SHOW CREATE TABLE;

SHOW GRANTS;

SHOW ERRORS;

SHOW WARNINGS;
```

## SELECT 检索数据

### 检索一列

```sql
select column_name from tb_xxx;
```

### 检索多个列

```sql
select column1,column2 from tb_xxx;
```

### 检索所有列
```sql
select * from tb_xxx;
```

### distinct 去重复的数据
```sql
#筛选出不重复的用户名；
select distinct user_name from tb_xxx; 
```
使用DISTINCT关键字，它必须直接放在列名的前面；

DISTINCT关键字应用于所有列而不仅是前置它的列。如果给出SELECT DISTINCT vend_id,prod_price，除非指定的两个列都不同，否则所有行都将被检索出来。

### limit 限制结果
```sql
#最多返回5行
select prod_name from tb_product limit 5;

#从第5行开始，检索5行
select prod_name from tb_product limit 5,5;
select prod_name from tb_product limit 5 offset 5;

##检索第二行
select prod_name from tb_product limit 1,1;
```

### 完全限制表名和列名
```
select products.prod_name from crashcourse.products;
```

### order by排序
```sql
#按某一列排序
select prod_name from tb_product order by prod_name;

#按多个列排序：先按第一个列，然后按第二个列排序
select prod_id,prod_price,prod_name from tb_product order by prod_price,prod_name;
```

### desc降序排列
```sql
#按价格降序排列
select prod_id,prod_price,prod_name from tb_product order by prod_price desc;

#先按产品价格降序排列，然后对产品名升序排序
select prod_id,prod_price,prod_name from tb_product order by prod_price desc, prod_name;
```

DESC关键字只应用到直接位于其前面的列名。在上例中，只对prod_price列指定DESC，对prod_name列不指定。因此，prod_price列以降序排序，而prod_name列（在每个价格内）仍然按标准的升序排序。

在多个列上降序排序 如果想在多个列上进行降序排序，必须对每个列指定DESC关键字。

与DESC相反的关键字是ASC（ASCENDING），在升序排序时可以指定它。但实际上，ASC没有多大用处，因为升序是默认的（如果既不指定ASC也不指定DESC，则假定为ASC）。

## Where过滤数据