
## redis数据类型

Redis是一个开放源代码（BSD许可）的内存中数据结构存储，用作数据库，缓存和消息代理。

它支持数据结构，例如字符串，哈希，列表，集合，带范围查询的排序集合，位图，超日志，带有半径查询和流的地理空间索引。

Redis具有内置的复制，Lua脚本，LRU驱逐，事务和不同级别的磁盘持久性，并通过Redis Sentinel和Redis Cluster自动分区提供了高可用性。

### 基本数据类型

####  String 字符串类型

String是redis最基本的类型，你可以理解成Memcached一模一样的类型，一个key对应一个value。

String类型是二进制安全的，意思是redis的string可以包含任何数据，比如jpg图片或者序列化的对象。

String类型是redis最基本的数据类型，一个redis中字符串value最多可以是512M。

* 查看所有的key keys *

```
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name qinjiang
OK
127.0.0.1:6379> keys *
1) "name"

```

* 判断某个key是否存在 exists

```
127.0.0.1:6379> EXISTS name
(integer) 1
127.0.0.1:6379> EXISTS name1
(integer) 0

```





#### list 列表类型

#### set 集合类型

### Zset 有序集合类型

### hash类型



