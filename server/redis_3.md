
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


# mget Mget 命令返回所有(一个或多个)给定 key 的值。


# 如果给定的 key 里面，有某个 key 不存在，那么这个 key 返回特殊值 nil 。

# msetnx 当所有 key 都成功设置，返回 1 。
# 如果所有给定 key 都设置失败(至少有一个 key 已经存在)，那么返回 0 。原子操
作

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

### hash类型



