
## redis是什么

Redis：REmote DIctionary Server（远程字典服务器）

>是完全开源免费的，用C语言编写的，遵守BSD协议，是一个高性能的（Key/Value）分布式内存数据
库，基于内存运行，并支持持久化的NoSQL数据库，是当前最热门的NoSQL数据库之一，也被人们称为
数据结构服务器

Redis与其他key-value缓存产品有以下三个特点

Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使
用。

Redis不仅仅支持简单的 key-value 类型的数据，同时还提供list、set、zset、hash等数据结构的存
储。

Redis支持数据的备份，即master-slave模式的数据备份。

## redis的作用

内存存储和持久化：redis支持异步将内存中的数据写到硬盘上，同时不影响继续服务.

取最新N个数据的操作，如：可以将最新的10条评论的ID放在Redis的List集合里面.

发布、订阅消息系统.

地图信息分析.

定时器、计数器.

......




## 官方网站

https://redis.io/ 官网

http://www.redis.cn 中文网