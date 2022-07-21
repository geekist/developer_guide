- [一、数据库基本概念](#一数据库基本概念)
  - [1.1、术语](#11术语)
  - [1.2 MySQL介绍](#12-mysql介绍)
- [二、MySQL数据库操作](#二mysql数据库操作)
  - [2.1 连接数据库](#21-连接数据库)
  - [2.2 打开数据库 USE](#22-打开数据库-use)
  - [2.3 查看数据库和表 SHOW](#23-查看数据库和表-show)
- [三、mysql 表操作](#三mysql-表操作)
  - [3.1 创建表 CREATE TABLE](#31-创建表-create-table)
  - [3.2 更新表 ALTER TABLE](#32-更新表-alter-table)
  - [3.3 删除表 DROP TABLE](#33-删除表-drop-table)
  - [3.4 重命名表 RENAME TABLE](#34-重命名表-rename-table)
- [四、MYSQL数据类型](#四mysql数据类型)
  - [4.1 字符串类型](#41-字符串类型)
  - [4.2 数值类型](#42-数值类型)
  - [4.3 日期和时间类型](#43-日期和时间类型)
  - [4.4 二进制类型](#44-二进制类型)
- [五、数据库表的增删改查](#五数据库表的增删改查)
  - [5.1 添加数据 INSERT](#51-添加数据-insert)
    - [插入完整的行](#插入完整的行)
    - [插入多个行](#插入多个行)
    - [INSERT INTO插入检索出的数据](#insert-into插入检索出的数据)
  - [5.2 删除数据 DELETE FROM](#52-删除数据-delete-from)
    - [删除特定的行](#删除特定的行)
    - [删除所有的行](#删除所有的行)
  - [5.3 修改数据 UPDATE](#53-修改数据-update)
    - [修改指定的行](#修改指定的行)
    - [修改所有的行](#修改所有的行)
    - [删除列](#删除列)
  - [5.4 检索数据 SELECT](#54-检索数据-select)
    - [检索一列](#检索一列)
    - [检索多个列](#检索多个列)
    - [检索所有列](#检索所有列)
    - [distinct 去重复的数据](#distinct-去重复的数据)
    - [limit 限制结果](#limit-限制结果)
    - [完全限制表名和列名](#完全限制表名和列名)
    - [order by排序](#order-by排序)
    - [desc降序排列](#desc降序排列)
    - [Where过滤数据](#where过滤数据)
      - [=](#)
      - [<](#-1)
      - [>](#-2)
      - [<=](#-3)
      - [>=](#-4)
      - [<>](#-5)
      - [BETWEEN](#between)
      - [IS NULL](#is-null)
      - [AND](#and)
      - [OR](#or)
      - [AND和OR的优先级](#and和or的优先级)
      - [IN](#in)
      - [NOT IN](#not-in)
      - [LIKE + %通配符](#like--通配符)
      - [LIKE + _通配符](#like--_通配符)
      - [REGEXP 正则表达式](#regexp-正则表达式)
    - [计算字段](#计算字段)
    - [Concat函数实现拼接字段](#concat函数实现拼接字段)
    - [使用别名](#使用别名)
    - [函数](#函数)
      - [常用数值处理函数](#常用数值处理函数)
      - [文本处理函数](#文本处理函数)
      - [常用日期和时间处理函数](#常用日期和时间处理函数)
    - [聚集函数](#聚集函数)
      - [AVG()函数](#avg函数)
      - [COUNT（）函数](#count函数)
      - [MAX()函数](#max函数)
      - [MIN()函数](#min函数)
      - [SUM()函数](#sum函数)
    - [数据分组](#数据分组)
      - [GROUP BY](#group-by)
      - [HAVING过滤分组](#having过滤分组)
    - [select 子句的顺序](#select-子句的顺序)
    - [select子查询](#select子查询)
    - [join联结表](#join联结表)
      - [分表存储数据](#分表存储数据)
      - [内联 INNER JOIN](#内联-inner-join)
      - [左连接 LEFT JOIN](#左连接-left-join)
      - [右连接 RIGHT JOIN](#右连接-right-join)
      - [外联结 OUTER JOIN](#外联结-outer-join)
    - [组合查询 UNION](#组合查询-union)
- [其他：存储过程、数据库安全、数据库性能、数据库维护等](#其他存储过程数据库安全数据库性能数据库维护等)

# 一、数据库基本概念

## 1.1、术语

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

## 1.2 MySQL介绍

MySQL是一种DBMS，即它是一种数据库软件。

MySQL已经存在很久了，它在世界范围内得到了广泛的安装和使用。为什么有那么多的公司和开发人员使用MySQL？以下列出其原因。

 成本——MySQL是开放源代码的，一般可以免费使用（甚至可免费修改）。

 性能——MySQL执行很快（非常快）。

 可信赖——某些非常重要和声望很高的公司、站点使用MySQL，这些公司和站点都用MySQL来处理自己的重要数据。

 简单——MySQL很容易安装和使用。

事实上，MySQL受到的唯一真正的批评是它并不总是支持其他DBMS提供的功能和特性。然而，这一点也正在逐步得到改善，MySQL的各个新版本正不断增加新特性、新功能。

# 二、MySQL数据库操作

## 2.1 连接数据库

为了连接到MySQL，需要以下信息：

- 主机名（计算机名）——如果连接到本地MySQL服务器，为localhost；
  
- 端口（如果使用默认端口3306之外的端口）；
  
- 一个合法的用户名；
  
- 用户口令（如果需要）。

## 2.2 打开数据库 USE  

```sql
use database_name;
```

## 2.3 查看数据库和表 SHOW 

```sql
#列出所有的数据库
show databases;

#列出某数据库的所有表
show tables;

#列出某个表的所有列
show columns from tb_xxx;

#查看数据库状态
SHOW STATUS;

#查看创建数据库的sql
SHOW CREATE DATABASE;

#查看创建数据库表的sql
SHOW CREATE TABLE;

#查看权限
SHOW GRANTS;

#查看错误
SHOW ERRORS;

#查看警告
SHOW WARNINGS;
```

# 三、mysql 表操作

## 3.1 创建表 CREATE TABLE

用CREATE TABLE创建表，必须给出下列信息：

* 新表的名字，在关键字CREATE TABLE之后给出；

* 表列的名字和定义，用逗号分隔。

* 使用NULL或 NOT NULL，限定该列的值是空值或非空值。

* PRIMARY KEY 主键。表中的每一行必须有主键值。如果主键使用单个列，它的值必须唯一。如果使用多个列，这些列的组合值必须唯一。

* AUTO_INCREMWNT 告诉mysql，本列每当增加一行时自动增量。每个表只允许一个AUTO_INCREMENT列，且它必须被索引。主键自动增加的一个不足是你不知道生成的id是多少，但是可以用last_insert_id()函数来获得这个值。

* DEFAULT 默认值。 在没有给出数量的情况下，可以使用默认给出的数量。

* 引擎类型：
InnoDB是一个可靠的事务处理引擎，它不支持全文本搜索；MEMORY在功能等同于MyISAM，但由于数据存储在内存（不是磁盘）中，速度很快（特别适合于临时表）；MyISAM是一个性能极高的引擎，它支持全文本搜索（参见第18章），但不支持事务处理。

```sql
create table u_order
(
    id        int NOT NULL AUTO_INCREMENT,
    order_num int NOT NULL,
    order_item int NOT NULL,
    prod_id char(10) NOT NULL,
    quantity int NOT NULL,
    item_price decimal(8,2) NOT NULL,
    PRIMARY KEY (order_num,order_item)
) ENGINE=InnoDB;
```
## 3.2 更新表 ALTER TABLE

为了使用ALTER TABLE更改表结构，必须给出下面的信息：

在ALTER TABLE之后给出要更改的表名（该表必须存在，否则将出错）；

所做更改的列表。
```sql
alert table vendors add vend_phone char(20);

alert table vendors drop column vend_phone;

```

## 3.3 删除表 DROP TABLE

```sql
drop table u_user;
```

## 3.4 重命名表 RENAME TABLE

```sql
rename table u_user to u_super_user;
```

# 四、MYSQL数据类型

数据类型是定义列中可以存储什么数据以及该数据实际怎样存储的基本规则。

数据类型用于以下目的：

- 数据类型允许限制可存储在列中的数据。例如，数值数据类型列只能接受数值。
  
- 数据类型允许在内部更有效地存储数据。可以用一种比文本串更简洁的格式存储数值和日期时间值。
  
- 数据类型允许变换排序顺序。如果所有数据都作为串处理，则1位于10之前，而10又位于2之前（串以字典顺序排序，从左边开始比较，一次一个字符）。作为数值数据类型，数值才能正确排序。

在设计表时，应该特别重视所用的数据类型。使用错误的数据类型可能会严重地影响应用程序的功能和性能。更改包含数据的列不是一件小事（而且这样做可能会导致数据丢失）。

## 4.1 字符串类型

|数据类型 |说 明|
| ---- | ---- |
| CHAR | 1～255个字符的定长串。它的长度必须在创建时指定，否则MySQL|假定为CHAR(1)|
| ENUM | 接受最多64 K个串组成的一个预定义集合的某个串|
| LONGTEXT| 与TEXT相同，但最大长度为4 GB|
| MEDIUMTEXT | 与TEXT相同，但最大长度为16 K|
| SET | 接受最多64个串组成的一个预定义集合的零个或多个串 |
| TEXT | 最大长度为64 K的变长文本 | 
| TINYTEXT | 与TEXT相同，但最大长度为255字节|
|VARCHAR | 长度可变，最多不超过255字节。如果在创建时指定为VARCHAR(n)，则可存储0到n个字符的变长串（其中n≤255）|


## 4.2 数值类型

|数据类型 |说 明|
| ---- | ---- |
|BIT |位字段，1～64位。（在MySQL 5之前，BIT在功能上等价于TINYINT|
|BIGINT |整数值，支持9223372036854775808～9223372036854775807（如果是UNSIGNED，为0～18446744073709551615）的数|
|BOOLEAN（或BOOL）| 布尔标志，或者为0或者为1，主要用于开/关（on/off）标志|
|DECIMAL（或DEC）| 精度可变的浮点值|
|DOUBLE |双精度浮点值|
|FLOAT|单精度浮点值|
|INT（或INTEGER）| 整数值，支持2147483648～2147483647（如果是UNSIGNED，为0～4294967295）的数|
|MEDIUMINT |整数值，支持8388608～8388607（如果是UNSIGNED，为0～16777215）的数|
|REAL |4字节的浮点值|
|SMALLINT| 整数值，支持32768～32767（如果是UNSIGNED，为0～65535）的数|
|TINYINT |整数值，支持128～127（如果为UNSIGNED，为0～255）的数|

有符号或无符号
 所有数值数据类型（除BIT和BOOLEAN外）都可以有符号或无符号。有符号数值列可以存储正或负的数值，无符号数值列只能存储正数。默认情况为有符号，但如果你知道自己不要存储负值，可以使用UNSIGNED关键字，这样做将允许你存储两倍大小的值。

 不使用引号
与串不一样，数值不应该括在引号内。

* BOOLEAN类型

MySQL没有内置的布尔类型。 但是它使用TINYINT(1)。 为了更方便，MySQL提供BOOLEAN或BOOL作为TINYINT(1)的同义词在MySQL中，0被认为是false，非零值被认为是true。 要使用布尔文本，可以使用常量TRUE和FALSE来分别计算为1和0。

在表中创建boolean类型的列
```sql
USE testdb;
CREATE TABLE tasks (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255) NOT NULL,
    completed BOOLEAN
    #completed TINYINT(1)
);
```
向tasts表中插入2行数据：可以插入true和false

```sql
INSERT INTO tasks(title,completed) 
VALUES('Master MySQL Boolean type',
        true),
      ('Design database table',
        false);
```

查询出来的结果是 0 和 1

```sql
select * from task;
```

where子句中作为boolean值来判断：

筛选出completed = 1的数据列表；
```sql
SELECT 
    id, title, completed
FROM
    tasks
WHERE
    completed = TRUE;
```
筛选出completed > 0 的数据列表；
```sql
SELECT 
    id, title, completed
FROM
    tasks
WHERE
    completed is TRUE;
```

存储货币数据类型 
MySQL中没有专门存储货币的数据类型，一般情况下使用DECIMAL(8, 2)

## 4.3 日期和时间类型

| 数据类型 | 说 明 |
| ---- | ---- |
| DATE | 表示1000-01-01～9999-12-31的日期，格式为YYYY-MM-DD|
| DATETIME | DATE和TIME的组合 |
| TIMESTAMP |功能和DATETIME相同（但范围较小）|
| TIME | 格式为HH:MM:SS|
|YEAR | 用2位数字表示，范围是70（1970年）～69（2069年），用4位数字表示，范围是1901年～2155年|

## 4.4 二进制类型

| 数据类型 | 说 明 |
| ---- | ---- |
| BLOB |Blob最大长度为64 KB |
| MEDIUMBLOB | Blob最大长度为16 MB|
| LONGBLOB | Blob最大长度为4 GB |
| TINYBLOB | Blob最大长度为255字节 |

# 五、数据库表的增删改查

## 5.1 添加数据 INSERT 

INSERT是用来插入（或添加）行到数据库表的。

### 插入完整的行

数据插入表中的最简单方法是使用基本的insert语句，要求指定表明和被插入到新行中的值。
```sql
insert into customers
values (NULL,
'Pep E.lapew',
233,
'CA',
'90046',
NULL,
NULL
);
```
更安全的方法是将列名和值都列出来：

```sql
insert into customers(cust_address,
cust_city,
cust_state,
cust_zip,
cust_country,
cust_month)
values(NULL,
'Pep E.lapew',
233,
'CA',
'90046',
NULL,
NULL
);
```
在括号里明确给出了列名，在插入行时，mysql将values列表中的对应值填入到列表中的对应项。因为提供了列名，values必须以其指定的次序匹配指定的列名，不一定按照各个列出现在实际表中的次序。

### 插入多个行

```sql
insert into customers(cust_name,
cust_address,
cust_city)
values(
    '1',
    '2',
    '3'
),
(
    '4',
    '5',
    '6'
);
```

### INSERT INTO插入检索出的数据

```sql
insert into customers(cust_id,
cust_contact,
cust_email,
cust_name)
select cust_id,
cust_contact,
cust_email,
cust_name
from custnew;
```

## 5.2 删除数据 DELETE FROM

### 删除特定的行
```sql
delete from customers where cust_id = 10005;
```
### 删除所有的行

```sql
delete from customers;
```

## 5.3 修改数据 UPDATE 

UPDATE语句非常容易使用，甚至可以说是太容易使用了。基本的UPDATE语句由3部分组成，分别是：

要更新的表；

列名和它们的新值；

确定要更新行的过滤条件。

### 修改指定的行

```sql
update customers
set cust_name = 'hello',
cust_email = 'wang@fan.com'
where cust_id = 10005;
```

### 修改所有的行

```sql
update customers
set cust_name = 'hello',
cust_email = 'wang@fan.com'
```

### 删除列
```sql
update customers
set cust_name = NULL，
cust_email = NULL
```

## 5.4 检索数据 SELECT 

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

#从行5开始，检索5行
select prod_name from tb_product limit 5,5;
select prod_name from tb_product limit 5 offset 5;

##检索第二行
select prod_name from tb_product limit 1,1;
```

### 完全限制表名和列名
```sql
select products.prod_name from crashcourse.products;
```

### order by排序
```sql
#按某一列排序
select prod_name from tb_product order by prod_name;

#按多个列排序：先按第一个列，然后按第二个列排序
select prod_id,prod_price,prod_name from tb_product order by prod_price,prod_name;

#order by 和limit组合，找出最高值
select prod_price from products order by prod_price desc limit 1;
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

###  Where过滤数据

数据库表一般包含大量的数据，很少需要检索表中所有行。通常只会根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要指定搜索条件（search criteria），搜索条件也称为过滤条件（filtercondition）。

在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。WHERE子句在表名（FROM子句）之后给出.

where子句操作符

| 操作符 | 说明 |
| ---- | ----|
| = | 等于|
| <> |不等于 |
| != | 不等于|
| < | 小于|
| <= | 小于等于|
| > | 大于|
| >= | 大于等于|
| BETWEEN | 在两个指定的值之间|

####  =
```sql
select prod_name,prod_price from tb_product where prod_name = 'false';
```

#### <
```sql
select prod_name,prod_price from tb_product where prod_price < 10;
```

#### >
```sql
seleect prod_name,prod_price from tb_product where prod_price > 10;
```

#### <=
```sql
select prod_name, prod_price from tb_product where prod_price <= 10;
```

#### >=
```sql
select prod_name, prod_price from tb_product where prod_price >= 10;
```

#### <>
```sql
select vend_id,prod_name from tb_product where vend_id <> 1003;
```

#### BETWEEN
```sql
select vend_id,prod_name from tb_product where vend_id between  1003 and 1013;
```

从这个例子中可以看到，在使用BETWEEN时，必须指定两个值——所需范围的低端值和高端值。这两个值必须用AND关键字分隔。BETWEEN匹配范围中所有的值，包括指定的开始值和结束值。

#### IS NULL
```sql
select prod_name from products where prod_price IS NULL;
```

#### AND 
```sql
select prod_name, prod_price where vend_id = 1003 and prod_price <= 10;
```

#### OR
```sql
select prod_name, prod_price where vend_id = 1003 or prod_price <= 10;
```

#### AND和OR的优先级
WHERE可包含任意数目的AND和OR操作符。允许两者结合以进行复杂和高级的过滤。

SQL语句优先处理AND操作符，然后处理OR操作符，如果需要OR操作符优先进行处理，需要用圆括号括起来
```sql
select prod_name,prod_price from tb_product where (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
```

#### IN
```sql
select prod_name,prod_price from tb_product where vend_id IN (1002,1003) order by prod_name;
```

#### NOT IN
```sql
select prod_name,prod_price from tb_product where vend_id NOT IN (1002,1003) order by prod_name;
```

#### LIKE + %通配符

```sql
select pord_id, prod_name from tb_product where prod_name LIKE 'jet%'
```
最常使用的通配符是百分号（%）。在搜索串中，%表示任何字符出现任意次数。上例表示任意以jet开头的字符串；

```sql
select pord_id, prod_name from tb_product where prod_name LIKE 'jet%x'
#搜索以jet开头，以x结尾的任意字符串
```
#### LIKE + _通配符
```sql
select prod_id, prod_name from tb_product where prod_name like '_ton';
#搜索以任意字符开头，ton结尾的字符串
```

不要过度使用通配符。如果其他操作符能达到相同的目的，应该使用其他操作符。

在确实需要使用通配符时，除非绝对有必要，否则不要把它们用在搜索模式的开始处。把通配符置于搜索模式的开始处，搜索起来是最慢的。

#### REGEXP 正则表达式
```sql
#查找prod_name包含文本1000的所有行
select prod_name from tb_product where prod_name REGEXP '1000'
```

### 计算字段

通过数据库的已有字段进行计算、组合或其他方式得到的新的字段，称为计算字段。

### Concat函数实现拼接字段
```sql
select Concat(vend_name,'(',vend_contry, ')') from tb_vendor order by vend_name; 
```
### 使用别名
```sql
select Concat(vend_name,'(',vend_contry, ')') as vend_title from tb_vendor order by vend_name; 
```
### 函数

#### 常用数值处理函数

|函 数 |说 明|
|---- | ---- |
|Abs() |返回一个数的绝对值|
|Cos() |返回一个角度的余弦|
|Exp() |返回一个数的指数值|
|Mod() |返回除操作的余数|
|Pi() |返回圆周率|
|Rand() |返回一个随机数|
|Sin() |返回一个角度的正弦|
|Sqrt() |返回一个数的平方根|
|Tan() |返回一个角度的正切|

#### 文本处理函数

|函 数 |说 明|
|---- | ---- |
|Left() |返回串左边的字符|
|Length() |返回串的长度|
|Locate() |找出串的一个子串|
|Lower() |将串转换为小写|
|LTrim() |去掉串左边的空格|
|Right() |返回串右边的字符|
|RTrim() |去掉串右边的空格|
|Soundex() |返回串的SOUNDEX值|
|SubString()| 返回子串的字符|
|Upper()| 将串转换为大写|

#### 常用日期和时间处理函数

|函 数 |说 明|
|---- | ---- |
|AddDate() |增加一个日期（天、周等）|
|AddTime() |增加一个时间（时、分等）|
|CurDate() |返回当前日期|
|CurTime()| 返回当前时间|
|Date() |返回日期时间的日期部分|
|DateDiff() |计算两个日期之差|
|Date_Add() |高度灵活的日期运算函数|
|Date_Format()| 返回一个格式化的日期或时间串|
|Day() |返回一个日期的天数部分|
|DayOfWeek()| 对于一个日期，返回对应的星期几|
|Hour() |返回一个时间的小时部分|
|Minute()| 返回一个时间的分钟部分|
|Month()| 返回一个日期的月份部分|
|Now() |返回当前日期和时间|
|Second()| 返回一个时间的秒部分|
|Time() |返回一个日期时间的时间部分|
|Year() |返回一个日期的年份部分|

DATE_FORMAT() 函数
DATE_FORMAT() 函数用于以不同的格式显示日期/时间数据。
语法
```sql
DATE_FORMAT(date,format)
#date 参数是合法的日期。format 规定日期/时间的输出格式。
```
常用的格式输出为：
|格式	|描述|
| ---- | ---- |
|%a	|缩写星期名|
|%b	|缩写月名|
|%c	|月，数值|
|%D	|带有英文前缀的月中的天|
|%d	|月的天，数值(00-31)|
|%e	|月的天，数值(0-31)|
|%f	|微秒|
|%H	|小时 (00-23)|
|%h	|小时 (01-12)|
|%I	|小时 (01-12)|
|%i	|分钟，数值(00-59)|
|%j	|年的天 (001-366)|
|%k	|小时 (0-23)|
|%l	|小时 (1-12)|
|%M	|月名|
|%m	|月，数值(00-12)|
|%p	|AM 或 PM|
|%r	|时间，12-小时（hh:mm:ss AM 或 PM）|
|%S	|秒(00-59)|
|%s	|秒(00-59)|
|%T	|时间, 24-小时 (hh:mm:ss)|
|%U	|周 (00-53) 星期日是一周的第一天|
|%u	|周 (00-53) 星期一是一周的第一天|
|%V	|周 (01-53) 星期日是一周的第一天，与 %X 使用|
|%v	|周 (01-53) 星期一是一周的第一天，与 %x 使用|
|%W	|星期名|
|%w	|周的天 （0=星期日, 6=星期六）|
|%X	|年，其中的星期日是周的第一天，4 位，与 %V 使用|
|%x	|年，其中的星期一是周的第一天，4 位，与 %v 使用|
|%Y	|年，4 位|
|%y	|年，2 位|

### 聚集函数

#### AVG()函数

AVG()通过对表中行数计数并计算特定列值之和，求得该列的平均值。AVG()可用来返回所有列的平均值，也可以用来返回特定列或行的平均值。

```sql
select avg(prod_price) as avg_price from tb_product;
```

```sql
#返回特定供应商提供产品的评价价格
select avg(prod_price) as avg_price from tb_product where vend_id = 1003;
```
上例中只过滤出vend_id为1003的产品。

#### COUNT（）函数

COUNT()函数进行计数。可利用COUNT()确定表中行的数目或符合特定条件的行的数目。

COUNT()函数有两种使用方式。

使用COUNT(*)对表中行的数目进行计数，不管表列中包含的是空值（NULL）还是非空值。

使用COUNT(column)对特定列中具有值的行进行计数，忽略NULL值。

```sql
#列出所有行的数目，不管行中各列有什么值
select count(*) as num_cust from customers;

#只计算某一列不为空的行数,本例中只计算cust_email不为空的值。
select count(cust_email) as num_cust from customers;
```

#### MAX()函数

MAX()返回指定列中的最大值。MAX()要求指定列名。MAX()一般用来找出最大的数值或日期值，但MySQL允许将它用来返回任意列中的最大值，包括返回文本列中的最大值。在用于文本数据时，如果数据按相应的列排序，则MAX()返回最后一行。

```sql
select max(prod_price) as max_price from products;
```

#### MIN()函数
返回指定列的最小值。MIN()要求指定列名。对非数值数据使用MIN() MIN()函数与MAX()函数类似，MySQL允许将它用来返回任意列中的最小值，包括返回文本列中的最小值。在用于文本数据时，如果数据按相应的列排序，则MIN()返回最前面的行。
```sql
#MIN()返回products表中最便宜物品的价格。
select min(prod_price) as min_price from products;
```

#### SUM()函数
返回指定列值的和（总计）。
```sql
select sum(quantity) as items_ordered from orderitems where order_num = 20005;
```

### 数据分组

#### GROUP BY

在具体使用GROUP BY子句前，需要知道一些重要的规定。
- GROUP BY子句可以包含任意数目的列。这使得能对分组进行嵌套，为数据分组提供更细致的控制。
  
- 如果在GROUP BY子句中嵌套了分组，数据将在最后规定的分组上进行汇总。换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别的列取回数据）。
  
- GROUP BY子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）。如果在SELECT中使用表达式，则必须在GROUP BY子句中指定相同的表达式。不能使用别名。

- 除聚集计算语句外，SELECT语句中的每个列都必须在GROUP BY子句中给出。

- 如果分组列中具有NULL值，则NULL将作为一个分组返回。如果列中有多行NULL值，它们将分为一组。

- GROUP BY子句必须出现在WHERE子句之后，ORDER BY子句之前。

```sql
select vend_id, count(*) as num_prods from tb_product group by vend_id;
```
结果如下：
|vend_id | num_prods|
| ---- | ---- |
| 1001 | 3 |
| 1002 | 2 |
| 1003 | 7 |
| 1005 | 2 |
上面的select语句指定了两个列，vend_id包含产品供应商的ID，num_prods为计算字段（用count（*）建立。group by子句只是mysql 按照vend_id排序并分组数据。这导致对每个vend_id而不是整表计算num_prods一次。从输出中可以看出，供应商1001有3个产品，供应商1002有两个产品，供应商1003有7个产品；供应商1005有2个产品。

#### HAVING过滤分组

因为where子句对整个行进行过滤而不是对分组进行过滤，即where没有分组概念。因此，MYSQL使用having来过滤分组。

```sql
select vend_id, count(*) as num_prods from tb_product
group by vend_id
having count(*) >= 3;
```
结果如下：
|vend_id | num_prods|
| ---- | ---- |
| 1001 | 3 |
| 1003 | 7 |

```sql
select vend_id, count(*) as num_prods from products
where prod_price >=10
group by vend_id
having count(*) >= 2;
```

第一行是使用了聚集函数的基本SELECT，它与前面的例子很相像。

WHERE子句过滤所有prod_price至少为10的行。

然后按vend_id分组数据，HAVING子句过滤计数为2或2以上的分组。

如果没有WHERE子句，将会多检索出两行（供应商1002，销售的所有产品价格都在10以下；供应商1001，销售3个产品，但只有一个产品的价格大于等于10）

### select 子句的顺序

在select语句中，必须遵循下面的语句顺序：

|子 句 |说 明 | 是否必须使用 |
| ---- | ---- | ---- |
|SELECT | 要返回的列或表达式 | 是 |
| FROM | 从中检索数据的表 | 仅在从表选择数据时使用 |
| WHERE |  行级过滤 | 否 |
| GROUP BY |分组说明  | 仅在按组计算聚集时使用 |
| HAVING | 组级过滤  | 否 |
| ORDER BY |输出排序顺序 | 否 |
| LIMIT | 要检索的行数 | 否 |

### select子查询

不使用子查询的例子：
```sql
select order_num from orderitems where prod_id = 'TNT2';

select cust_id from orders where order_num in(200005,200007);
```
合并后：
```sql
select cust_id from orders
where order_num in (select order_num from orderitems where prod_id = 'TNT2');
```
使用计算字段进行子查询：
```sql
select cust_name,
cust_state,
(select count(*) from orders where orders.cust_id = customer.cust_id) as orders
from customers
order by cust_name;
```
这 条 SELECT 语句对 customers 表中每个客户返回 3 列 ：cust_name、cust_state和orders。orders是一个计算字段，它是由圆括号中的子查询建立的。该子查询对检索出的每个客户执行一次。在此例子中，该子查询执行了5次，因为检索出了5个客户。

### join联结表

#### 分表存储数据

例子：在记录供应商数据和产品数据的时候可建立两个表，一个存储供应商信息，另一个存储产品信息。vendors表包含所有供应商信息，每个供应商占一行，每个供
应商具有唯一的标识。此标识称为主键（primary key可以是供应商ID或任何其他唯一值。

products表只存储产品信息，它除了存储供应商ID（vendors表的主键）外不存储其他供应商信息。vendors表的主键又叫作products的外键，它将vendors表与products表关联，利用供应商ID能从vendors表中找出相应供应商的详细信息。

#### 内联 INNER JOIN

```sql
select vend_name,prod_name,prod_price from vendors, products
where vendors.vend_id = products.vend_id
order by vend_name, prod_name;
```

```sql
SELECT a.vend_name,b.prod_name,b.prod_price  FROM vendors a INNER JOIN products b ON a.vend_id = b.vend_id;
```

#### 左连接 LEFT JOIN

左连接LEFT JOIN的含义就是求两个表的交集外加左表剩下的数据。依旧从笛卡尔积的角度讲，就是先从笛卡尔积中挑出ON子句条件成立的记录，然后加上左表中剩余的记录（见最后三条）。
```sql
SELECT * FROM t_blog LEFT JOIN t_type ON t_blog.typeId=t_type.id;
```
    +----+-------+--------+------+------+
    | id | title | typeId | id   | name |
    +----+-------+--------+------+------+
    |  1 | aaa   |      1 |    1 | C++  |
    |  2 | bbb   |      2 |    2 | C    |
    |  7 | ggg   |      2 |    2 | C    |
    |  3 | ccc   |      3 |    3 | Java |
    |  6 | fff   |      3 |    3 | Java |
    |  4 | ddd   |      4 |    4 | C#   |
    |  5 | eee   |      4 |    4 | C#   |
    |  8 | hhh   |   NULL | NULL | NULL |
    |  9 | iii   |   NULL | NULL | NULL |
    | 10 | jjj   |   NULL | NULL | NULL |
    +----+-------+--------+------+------+

#### 右连接 RIGHT JOIN 

同理右连接RIGHT JOIN就是求两个表的交集外加右表剩下的数据。再次从笛卡尔积的角度描述，右连接就是从笛卡尔积中挑出ON子句条件成立的记录，然后加上右表中剩余的记录（见最后一条）
```sql
SELECT * FROM t_blog RIGHT JOIN t_type ON t_blog.typeId=t_type.id;
```
    +------+-------+--------+----+------------+
    | id   | title | typeId | id | name       |
    +------+-------+--------+----+------------+
    |    1 | aaa   |      1 |  1 | C++        |
    |    2 | bbb   |      2 |  2 | C          |
    |    3 | ccc   |      3 |  3 | Java       |
    |    4 | ddd   |      4 |  4 | C#         |
    |    5 | eee   |      4 |  4 | C#         |
    |    6 | fff   |      3 |  3 | Java       |
    |    7 | ggg   |      2 |  2 | C          |
    | NULL | NULL  |   NULL |  5 | Javascript |
    +------+-------+--------+----+------------+

#### 外联结 OUTER JOIN 

```sql
    SELECT * FROM t_blog LEFT JOIN t_type ON t_blog.typeId=t_type.id
    UNION
    SELECT * FROM t_blog RIGHT JOIN t_type ON t_blog.typeId=t_type.id;
```

外连接就是求两个集合的并集。从笛卡尔积的角度讲就是从笛卡尔积中挑出ON子句条件成立的记录，然后加上左表中剩余的记录，最后加上右表中剩余的记录。另外MySQL不支持OUTER JOIN，但是我们可以对左连接和右连接的结果做UNION操作来实现。

    +------+-------+--------+------+------------+
    | id   | title | typeId | id   | name       |
    +------+-------+--------+------+------------+
    |    1 | aaa   |      1 |    1 | C++        |
    |    2 | bbb   |      2 |    2 | C          |
    |    7 | ggg   |      2 |    2 | C          |
    |    3 | ccc   |      3 |    3 | Java       |
    |    6 | fff   |      3 |    3 | Java       |
    |    4 | ddd   |      4 |    4 | C#         |
    |    5 | eee   |      4 |    4 | C#         |
    |    8 | hhh   |   NULL | NULL | NULL       |
    |    9 | iii   |   NULL | NULL | NULL       |
    |   10 | jjj   |   NULL | NULL | NULL       |
    | NULL | NULL  |   NULL |    5 | Javascript |
    +------+-------+--------+------+------------+

![](./assets/mysql_1.jpg)

### 组合查询 UNION 

多数SQL查询都只包含从一个或多个表中返回数据的单条SELECT语句。MySQL也允许执行多个查询（多条SELECT语句），并将结果作为单个查询结果集返回。这些组合查询通常称为并（union）或复合查询（compound query）。

有两种基本情况，其中需要使用组合查询：

- 在单个查询中从不同的表返回类似结构的数据；

- 对单个表执行多个查询，按单个查询返回数据。

```sql
select vend_id, prod_id,prod_price
from products
where prod_price <=5

union
select vend_id, prod_id, prod_price
from products
where vend_id in (1001,1002);
```

等价于
```sql
select vend_id, prod_id,prod_price
from products
where (prod_price <=5 or vend_id in (1001,1002));
```

# 其他：存储过程、数据库安全、数据库性能、数据库维护等
