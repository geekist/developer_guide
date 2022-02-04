
## redis数据类型

Redis是一个开放源代码（BSD许可）的内存中数据结构存储，用作数据库，缓存和消息代理。

它支持数据结构，例如字符串，哈希，列表，集合，带范围查询的排序集合，位图，超日志，带有半径查询和流的地理空间索引。

Redis具有内置的复制，Lua脚本，LRU驱逐，事务和不同级别的磁盘持久性，并通过Redis Sentinel和Redis Cluster自动分区提供了高可用性。

### 基本数据类型

####  String 字符串类型

String是redis最基本的类型，你可以理解成Memcached一模一样的类型，一个key对应一个value。

String类型是二进制安全的，意思是redis的string可以包含任何数据，比如jpg图片或者序列化的对象。

String类型是redis最基本的数据类型，一个redis中字符串value最多可以是512M。

* 给某个可以设置值： set key value

```
127.0.0.1:6379> set key1 value1 # 设置值
OK

```

* 获取某个key的值 get key

```
127.0.0.1:6379> get key1 # 获得key
"value1"

```

* 删除某个key del key

```
127.0.0.1:6379> del key1 # 删除key
(integer) 1

```

* 追加字符串 append key value2

```
127.0.0.1:6379> append key1 "hello" # 对不存在的 key 进行 APPEND ，等同于 SET
key1 "hello"
(integer) 5 # 字符长度
127.0.0.1:6379> APPEND key1 "-2333" # 对已存在的字符串进行 APPEND
(integer) 10 # 长度从 5 个字符增加到 10 个字符
127.0.0.1:6379> get key1
"hello-2333"
```

* 获取字符串长度

```
127.0.0.1:6379> STRLEN key1 # # 获取字符串的长度
(integer) 10

```

* value递增： incr

```
127.0.0.1:6379> set views 0 # 设置浏览量为0
OK
127.0.0.1:6379> incr views # 浏览 + 1
(integer) 1
127.0.0.1:6379> incr views # 浏览 + 1
(integer) 2
```

* value递减： decr

```
127.0.0.1:6379> decr views # 浏览 - 1
(integer) 1
```

* 按照步长递增 incrby

```
127.0.0.1:6379> incrby views 10 # +10
(integer) 11

```

* 按照步长递减 decrby

```
127.0.0.1:6379> decrby views 10 # -10
(integer) 1

```

*  getrange 获取指定区间范围内的值，类似between...and的关系，从0到-1表示全部

```
127.0.0.1:6379> set key2 abcd123456 # 设置key2的值
OK
127.0.0.1:6379> getrange key2 0 -1 # 获得全部的值
"abcd123456"
127.0.0.1:6379> getrange key2 0 2 # 截取部分字符串
"abc"
```
* setrange 设置指定区间范围内的值，格式是setrange key值 具体值

```
127.0.0.1:6379> get key2
"abcd123456"
127.0.0.1:6379> SETRANGE key2 1 xx # 替换值
(integer) 10
127.0.0.1:6379> get key2
"axxd123456"
```

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

* 为某个key设置生存时间  expire key seconds

```
127.0.0.1:6379> set name qinjiang
127.0.0.1:6379> EXPIRE name 10
(integer) 1

```

* ttl key 查看还有多少秒过期，-1 表示永不过期，-2 表示已过期

```
127.0.0.1:6379> EXPIRE name 10
(integer) 1
127.0.0.1:6379> ttl name
(integer) 4
127.0.0.1:6379> ttl name
```

* type key 查看你的key是什么类型

```
127.0.0.1:6379> set name qinjiang
OK
127.0.0.1:6379> get name
"qinjiang"
127.0.0.1:6379> type name
string

```


* setex（set with expire）键秒值

```
27.0.0.1:6379> setex key3 60 expire # 设置过期时间
OK
127.0.0.1:6379> ttl key3 # 查看剩余的时间
(integer) 55

```

* setnx（set if not exist）

```
127.0.0.1:6379> setnx mykey "redis" # 如果不存在就设置，成功返回1
(integer) 1
127.0.0.1:6379> setnx mykey "mongodb" # 如果存在就设置，失败返回0
(integer) 0
127.0.0.1:6379> get mykey
"redis"

```
* mset Mset 命令用于同时设置一个或多个 key-value 对。


* mget Mget 命令返回所有(一个或多个)给定 key 的值。

如果给定的 key 里面，有某个 key 不存在，那么这个 key 返回特殊值 nil 。

* msetnx 当所有 key 都成功设置，返回 1 。

如果所有给定 key 都设置失败(至少有一个 key 已经存在)，那么返回 0 。原子操作

```
127.0.0.1:6379> mset k10 v10 k11 v11 k12 v12
OK
127.0.0.1:6379> keys *
1) "k12"
2) "k11"
3) "k10"
127.0.0.1:6379> mget k10 k11 k12 k13
1) "v10"
2) "v11"
3) "v12"
4) (nil)
127.0.0.1:6379> msetnx k10 v10 k15 v15 # 原子性操作！
(integer) 0
127.0.0.1:6379> get key15
(nil)

```

* getset（先get再set）
* 
```
127.0.0.1:6379> getset db mongodb # 没有旧值，返回 nil
(nil)
127.0.0.1:6379> get db
"mongodb"
127.0.0.1:6379> getset db redis # 返回旧值 mongodb
"mongodb"
127.0.0.1:6379> get db
"redis"
```

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。



#### list 列表类型


*  Lpush：将一个或多个值插入到列表头部。（左）


* rpush：将一个或多个值插入到列表尾部。（右）


* lrange：返回列表中指定区间内的元素，区间以偏移量 START 和 END 指定。


其中 0 表示列表的第一个元素， 1 表示列表的第二个元素，以此类推。
你也可以使用负数下标，以 -1 表示列表的最后一个元素， -2 表示列表的倒数第二个元素，以此
类推。

```
127.0.0.1:6379> LPUSH list "one"
(integer) 1
127.0.0.1:6379> LPUSH list "two"
(integer) 2

127.0.0.1:6379> RPUSH list "right"
(integer) 3
127.0.0.1:6379> Lrange list 0 -1
1) "two"
2) "one"
3) "right"
127.0.0.1:6379> Lrange list 0 1
1) "two"
2) "one"
```

* lpop 命令用于移除并返回列表的第一个元素。当列表 key 不存在时，返回 nil 。


* rpop 移除列表的最后一个元素，返回值为移除的元素。


```
127.0.0.1:6379> Lpop list
"two"
127.0.0.1:6379> Rpop list
"right"
127.0.0.1:6379> Lrange list 0 -1
1) "one"
```

* Lindex，按照索引下标获得元素（-1代表最后一个，0代表是第一个）


```
127.0.0.1:6379> Lindex list 1
(nil)
127.0.0.1:6379> Lindex list 0
"one"
127.0.0.1:6379> Lindex list -1
"one"
```

* llen 用于返回列表的长度。


```
127.0.0.1:6379> flushdb
OK
127.0.0.1:6379> Lpush list "one"
(integer) 1
127.0.0.1:6379> Lpush list "two"
(integer) 2
127.0.0.1:6379> Lpush list "three"
(integer) 3
127.0.0.1:6379> Llen list # 返回列表的长度
(integer) 3
```


* lrem key 根据参数 COUNT 的值，移除列表中与参数 VALUE 相等的元素。


```
127.0.0.1:6379> lrem list 1 "two"
(integer) 1
127.0.0.1:6379> Lrange list 0 -1
1) "three"
2) "one"
```


* Ltrim key 对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区
间之内的元素都将被删除。

```
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 1
127.0.0.1:6379> RPUSH mylist "hello"
(integer) 2
127.0.0.1:6379> RPUSH mylist "hello2"
(integer) 3
127.0.0.1:6379> RPUSH mylist "hello3"
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2
OK
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello2"

```


* rpoplpush 移除列表的最后一个元素，并将该元素添加到另一个列表并返回。


```
127.0.0.1:6379> rpush mylist "hello"
(integer) 1
127.0.0.1:6379> rpush mylist "foo"
(integer) 2
127.0.0.1:6379> rpush mylist "bar"
(integer) 3
127.0.0.1:6379> rpoplpush mylist myotherlist
"bar"
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "foo"
127.0.0.1:6379> lrange myotherlist 0 -1
1) "bar"
```


* lset key index value 将列表 key 下标为 index 的元素的值设置为 value 。


```
127.0.0.1:6379> exists list # 对空列表(key 不存在)进行 LSET
(integer) 0
127.0.0.1:6379> lset list 0 item # 报错
(error) ERR no such key
127.0.0.1:6379> lpush list "value1" # 对非空列表进行 LSET
(integer) 1
127.0.0.1:6379> lrange list 0 0
1) "value1"
127.0.0.1:6379> lset list 0 "new" # 更新值
OK
127.0.0.1:6379> lrange list 0 0
1) "new"
127.0.0.1:6379> lset list 1 "new" # index 超出范围报错
(error) ERR index out of range
```

* linsert key before/after pivot value 用于在列表的元素前或者后插入元素。


将值 value 插入到列表 key 当中，位于值 pivot 之前或之后。

```
redis> RPUSH mylist "Hello"
(integer) 1
redis> RPUSH mylist "World"
(integer) 2
redis> LINSERT mylist BEFORE "World" "There"
(integer) 3
redis> LRANGE mylist 0 -1
1) "Hello"
2) "There"
3) "World"

```

list就是链表，略有数据结构知识的人都应该能理解其结构。使用Lists结构，我们可以轻松地实现最新消息排行等功能。

List的另一个应用就是消息队列，可以利用List的PUSH操作，将任务存在List中，然后工作线程再用POP操作将任务取出进行执行。

Redis还提供了操作List中某一段的api，你可以直接查询，删除List中某一段的元素。

Redis的list是每个子元素都是String类型的双向链表，可以通过push和pop操作从列表的头部或者尾部
添加或者删除元素，这样List即可以作为栈，也可以作为队列。

#### set 集合类型

>set是指无序的不重复的数组列表。


* sadd 将一个或多个成员元素加入到集合中，不能重复

* smembers 返回集合中的所有的成员。

* sismember 命令判断成员元素是否是集合的成员。

```
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 0
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
2) "hello"
127.0.0.1:6379> SISMEMBER myset "hello"
(integer) 1
127.0.0.1:6379> SISMEMBER myset "world"
(integer) 0
```


* scard，获取集合里面的元素个数

```
127.0.0.1:6379> scard myset
(integer) 2
```


* srem key value 用于移除集合中的一个或多个成员元素

```
127.0.0.1:6379> srem myset "kuangshen"
(integer) 1
```


* srandmember key 命令用于返回集合中的一个随机元素。

```
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
2) "world"
3) "hello"
127.0.0.1:6379> SRANDMEMBER myset
"hello"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "world"
2) "kuangshen"
127.0.0.1:6379> SRANDMEMBER myset 2
1) "kuangshen"
2) "hello"
```


* spop key 用于移除集合中的指定 key 的一个或多个随机元素

```
127.0.0.1:6379> SMEMBERS myset
1) "kuangshen"
2) "world"
3) "hello"
127.0.0.1:6379> spop myset
"world"
127.0.0.1:6379> spop myset
"kuangshen"
127.0.0.1:6379> spop myset
"hello"
```

* smove SOURCE DESTINATION MEMBER

将指定成员 member 元素从 source 集合移动到 destination 集合。

```
127.0.0.1:6379> sadd myset "hello"
(integer) 1
127.0.0.1:6379> sadd myset "world"
(integer) 1
127.0.0.1:6379> sadd myset "kuangshen"
(integer) 1
127.0.0.1:6379> sadd myset2 "set2"
(integer) 1
127.0.0.1:6379> smove myset myset2 "kuangshen"
(integer) 1
127.0.0.1:6379> SMEMBERS myset
1) "world"
2) "hello"
127.0.0.1:6379> SMEMBERS myset2
1) "kuangshen"
2) "set2"
```


* 差集： sdiff


* 交集： sinter

* 并集： sunion

```
127.0.0.1:6379> sadd key1 "a"
(integer) 1
127.0.0.1:6379> sadd key1 "b"
(integer) 1
127.0.0.1:6379> sadd key1 "c"
(integer) 1
127.0.0.1:6379> sadd key2 "c"
(integer) 1
127.0.0.1:6379> sadd key2 "d"
(integer) 1
127.0.0.1:6379> sadd key2 "e"
(integer) 1
127.0.0.1:6379> SDIFF key1 key2 # 差集
1) "a"
2) "b"
127.0.0.1:6379> SINTER key1 key2 # 交集
1) "c"
127.0.0.1:6379> SUNION key1 key2 # 并集
1) "a"
2) "b"
3) "c"
4) "e"
5) "d"
```

### Zset 有序集合类型

在set基础上，加一个score值。之前set是k1 v1 v2 v3，现在zset是 k1 score1 v1 score2 v2


* zadd 将一个或多个成员元素及其分数值加入到有序集当中。

* zrange 返回有序集中，指定区间内的成员

```
127.0.0.1:6379> zadd myset 1 "one"
(integer) 1
127.0.0.1:6379> zadd myset 2 "two" 3 "three"
(integer) 2
127.0.0.1:6379> ZRANGE myset 0 -1
1) "one"
2) "two"
3) "three"
```

* zrangebyscore 返回有序集合中指定分数区间的成员列表。有序集成员按分数值递增(从小到大)
次序排列。


```
127.0.0.1:6379> zadd salary 2500 xiaoming
(integer) 1
127.0.0.1:6379> zadd salary 5000 xiaohong
(integer) 1
127.0.0.1:6379> zadd salary 500 kuangshen
(integer) 1
# Inf无穷大量+∞,同样地,-∞可以表示为-Inf。
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf # 显示整个有序集
1) "kuangshen"
2) "xiaoming"
3) "xiaohong"
127.0.0.1:6379> ZRANGEBYSCORE salary -inf +inf withscores # 递增排列
1) "kuangshen"
2) "500"
3) "xiaoming"
4) "2500"
5) "xiaohong"
6) "5000"
```


* zrem 移除有序集中的一个或多个成员

```
127.0.0.1:6379> ZRANGE salary 0 -1
1) "kuangshen"
2) "xiaoming"
3) "xiaohong"
127.0.0.1:6379> zrem salary kuangshen
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1
1) "xiaoming"
2) "xiaohong"
```

* zcard 命令用于计算集合中元素的数量。

```
127.0.0.1:6379> zcard salary
(integer) 2
OK
```


* zcount 计算有序集合中指定分数区间的成员数量。

```
127.0.0.1:6379> zadd myset 1 "hello"
(integer) 1
127.0.0.1:6379> zadd myset 2 "world" 3 "kuangshen"
(integer) 2
127.0.0.1:6379> ZCOUNT myset 1 3
(integer) 3
127.0.0.1:6379> ZCOUNT myset 1 2
(integer) 2
```

* zrank 返回有序集中指定成员的排名。其中有序集成员按分数值递增(从小到大)顺序排列。

```
127.0.0.1:6379> zadd salary 2500 xiaoming
(integer) 1
127.0.0.1:6379> zadd salary 5000 xiaohong
(integer) 1
127.0.0.1:6379> zadd salary 500 kuangshen
(integer) 1
127.0.0.1:6379> ZRANGE salary 0 -1 WITHSCORES # 显示所有成员及其 score 值
1) "kuangshen"
2) "500"
3) "xiaoming"
4) "2500"
5) "xiaohong"
6) "5000"
127.0.0.1:6379> zrank salary kuangshen # 显示 kuangshen 的薪水排名，最少
(integer) 0
127.0.0.1:6379> zrank salary xiaohong # 显示 xiaohong 的薪水排名，第三
(integer) 2
```


* zrevrank 返回有序集中成员的排名。其中有序集成员按分数值递减(从大到小)排序。

```
127.0.0.1:6379> ZREVRANK salary kuangshen # 狂神第三
(integer) 2
127.0.0.1:6379> ZREVRANK salary xiaohong # 小红第一
(integer) 0
```



和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列，比如一个存储全班同学成绩的sorted set，其集合value可以是同学的学号，而score就可以是其考试得分，这样在数据插入集合的时候，就已经进行了天然的排序。

可以用sorted set来做带权重的队列，比如普通消息的score为1，重要消息的score为2，然后工作线程可以选择按score的倒序来获取工作任务。让重要的任务优先执行。

排行榜应用，取TOP N操作 ！

### hash类型

kv模式不变，但V是一个键值对


* hset、hget 命令用于为哈希表中的字段赋值 。

* hmset、hmget 同时将多个field-value对设置到哈希表中。会覆盖哈希表中已存在的字段。

* hgetall 用于返回哈希表中，所有的字段和值。

* hdel 用于删除哈希表 key 中的一个或多个指定字段

```
127.0.0.1:6379> hset myhash field1 "kuangshen"
(integer) 1
127.0.0.1:6379> hget myhash field1
"kuangshen"
127.0.0.1:6379> HMSET myhash field1 "Hello" field2 "World"
OK
127.0.0.1:6379> HGET myhash field1
"Hello"
127.0.0.1:6379> HGET myhash field2
"World"
127.0.0.1:6379> hgetall myhash
1) "field1"
2) "Hello"
3) "field2"
4) "World"
127.0.0.1:6379> HDEL myhash field1
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "field2"
2) "World"
```


* hlen 获取哈希表中字段的数量。

```
127.0.0.1:6379> hlen myhash
(integer) 1
127.0.0.1:6379> HMSET myhash field1 "Hello" field2 "World"
OK
127.0.0.1:6379> hlen myhash
(integer) 2
```

* hexists 查看哈希表的指定字段是否存在。

```
127.0.0.1:6379> hexists myhash field1
(integer) 1
127.0.0.1:6379> hexists myhash field3
(integer) 0
```

* hkeys 获取哈希表中的所有域（field）。

* hvals 返回哈希表所有域(field)的值。

```
127.0.0.1:6379> HKEYS myhash
1) "field2"
2) "field1"
127.0.0.1:6379> HVALS myhash
1) "World"
2) "Hello"
```

* hincrby 为哈希表中的字段值加上指定增量值。

```
127.0.0.1:6379> hset myhash field 5
(integer) 1
127.0.0.1:6379> HINCRBY myhash field 1
(integer) 6
127.0.0.1:6379> HINCRBY myhash field -1
(integer) 5
127.0.0.1:6379> HINCRBY myhash field -10
(integer) -5
```

* hsetnx 为哈希表中不存在的的字段赋值 。

```
127.0.0.1:6379> HSETNX myhash field1 "hello"
(integer) 1 # 设置成功，返回 1 。
127.0.0.1:6379> HSETNX myhash field1 "world"
(integer) 0 # 如果给定字段已经存在，返回 0 。
127.0.0.1:6379> HGET myhash field1
"hello"
```

### 特殊数据类型

#### GEO地理位置

Redis 的 GEO 特性在 Redis 3.2 版本中推出， 这个功能可以将用户给定的地理位置信息储存起来， 并对这些信息进行操作。来实现诸如附近位置、摇一摇这类依赖于地理位置信息的功能。geo的数据类型为
zset。

GEO 的数据结构总共有六个常用命令：geoadd、geopos、geodist、georadius、
georadiusbymember、gethash

官方文档：https://www.redis.net.cn/order/3685.html



#### HyperLogLog

Redis 在 2.8.9 版本添加了 HyperLogLog 结构。

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积
非常非常大时，计算基数所需的空间总是固定 的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

HyperLogLog则是一种算法，它提供了不精确的去重计数方案。

举个栗子：假如我要统计网页的UV（浏览用户数量，一天内同一个用户多次访问只能算一次），传统的
解决方案是使用Set来保存用户id，然后统计Set中的元素数量来获取页面UV。但这种方案只能承载少量
用户，一旦用户数量大起来就需要消耗大量的空间来存储用户id。我的目的是统计用户数量而不是保存
用户，这简直是个吃力不讨好的方案！而使用Redis的HyperLogLog最多需要12k就可以统计大量的用户
数，尽管它大概有0.81%的错误率，但对于统计UV这种不需要很精确的数据是可以忽略不计的。

>什么是基数？
>比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(不重复元素)为5。
>
>基数估计就是在误差可接受的范围内，快速计算基数。



* [PFADD key element [element ...] 添加指定元素到 HyperLogLog 中。

* [PFCOUNT key [key ...] 返回给定 HyperLogLog 的基数估算值。

* [PFMERGE destkey sourcekey [sourcekey ...] 将多个 HyperLogLog 合并为一个HyperLogLog，并集计算

```
127.0.0.1:6379> PFADD mykey a b c d e f g h i j
1
127.0.0.1:6379> PFCOUNT mykey
10
127.0.0.1:6379> PFADD mykey2 i j z x c v b n m
1
127.0.0.1:6379> PFMERGE mykey3 mykey mykey2
OK
127.0.0.1:6379> PFCOUNT mykey3
15

```
#### BitMap

在开发中，可能会遇到这种情况：需要统计用户的某些信息，如活跃或不活跃，登录或者不登录；又如
需要记录用户一年的打卡情况，打卡了是1， 没有打卡是0，如果使用普通的 key/value存储，则要记录
365条记录，如果用户量很大，需要的空间也会很大，所以 Redis 提供了 Bitmap 位图这中数据结构，
Bitmap 就是通过操作二进制位来进行记录，即为 0 和 1；如果要记录 365 天的打卡情况，使用 Bitmap表示的形式大概如下：0101000111000111...........................，这样有什么好处呢？当然就是节约内存了，365 天相当于 365 bit，又 1 字节 = 8 bit , 所以相当于使用 46 个字节即可。

BitMap 就是通过一个 bit 位来表示某个元素对应的值或者状态, 其中的 key 就是对应元素本身，实际上底层也是通过对字符串的操作来实现。

Redis 从 2.2 版本之后新增了setbit, getbit, bitcount 等几个bitmap 相关命令。

* setbit 设置操作

SETBIT key offset value : 设置 key 的第 offset 位为value (1或0)



使用 bitmap 来记录上述事例中一周的打卡记录如下所示：
周一：1，周二：0，周三：0，周四：1，周五：1，周六：0，周天：0 （1 为打卡，0 为不打卡）


```
127.0.0.1:6379> setbit sign 0 1
0
127.0.0.1:6379> setbit sign 1 0
0
127.0.0.1:6379> setbit sign 2 0
0
127.0.0.1:6379> setbit sign 3 1
0
127.0.0.1:6379> setbit sign 4 1
0
127.0.0.1:6379> setbit sign 5 0
0
127.0.0.1:6379> setbit sign 6 0
0
```

* getbit 获取操作

GETBIT key offset 获取offset设置的值，未设置过默认返回0

```
127.0.0.1:6379> getbit sign 3 # 查看周四是否打卡
1
127.0.0.1:6379> getbit sign 6 # 查看周七是否打卡
0
```

* bitcount 统计操作

bitcount key [start, end] 统计 key 上位为1的个数

 统计这周打卡的记录，可以看到只有3天是打卡的状态：

```
127.0.0.1:6379> bitcount sign
3
```