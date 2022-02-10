

## 安装redis

### 1: 从[redis官方网站](https://redis.io/)下载压缩包redis-6.2.6.tar.gz 后将它放到我们Linux的目录下 /opt


### 2、在/opt 目录下，解压该压缩包

命令 ： 
```
tar -zxvf redis-6.2.6.tar.gz
```

### 3、解压完成后出现文件夹：redis-6.2.6

### 4、进入目录： cd redis-6.2.6


### 5、在 redis-6.2.6 目录下执行 make 命令

如果运行make命令时出现错误，可能是没有安装编译环境：

  安装gcc (gcc是linux下的一个编译程序，是c程序的编译工具)

  首先系统连接上网络，执行：
   yum install gcc-c++

   版本测试: gcc-v

   然后再次执行make

### 6、make完成后继续执行 make install

    
### 7、查看默认安装目录：usr/local/bin

>/usr 这是一个非常重要的目录，类似于windows下的Program Files,存放用户的程序.


### 8、拷贝一个配置文件（***备用，作为redis的启动参数***）

```
cd /usr/local/bin
ls -l
# 在redis的解压目录下备份redis.conf
mkdir dredis
cp opt/redis6.2.6/redis.conf dredis/redis.conf 

# 拷一个备份，养成良好的习惯，我们就修改这个文件
```

#### 9、修改配置保证可以后台应用

vim redis.conf

在redids.conf中找到dameonize的配置，设置为yes

A、redis.conf配置文件中daemonize守护线程，默认是NO。

B、daemonize是用来指定redis是否要用守护线程的方式启动。

>daemonize 设置yes或者no区别
>
>daemonize:yes
>
>redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启
守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项
pidfile设置的文件中，此时redis将一直运行，除非手动kill该进程。
>
>daemonize:no
>
>当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭
连接工具(putty,xshell等)都会导致redis进程退出。

## 使用redis

### 启动redis服务器

```

cd /usr/bin

redis-server dredis/redis.conf

```

### 启动redis客户端

```
redis-cli -h localhost -p 6379

127.0.0.1:6379>ping

pong

```

### 查看redis是否启动 ps -ef|grep redis

```
[root@192 myredis]# ps -ef|grep redis
root 16005 1 0 04:45 ? 00:00:00 redis-server
127.0.0.1:6379
root 16031 15692 0 04:47 pts/0 00:00:00 redis-cli -p 6379
root 16107 16076 0 04:51 pts/2 00:00:00 grep --color=auto redis
```

### 关闭redis连接 shutdown 

```
127.0.0.1:6379> shutdown
not connected> exit
```

再次使用ps命令显示系统当前进程 
ps -ef|grep redis

```
[root@192 myredis]# ps -ef|grep redis
root 16140 16076 0 04:53 pts/2 00:00:00 grep --color=auto redis
```

## redis数据库基本命令

redis默认有16个数据库，类似数组下标从零开始，初始默认使用零号库。

在redis.conf中有默认配置，设置为16


```
# Set the number of databases. The default database is DB 0, you can select
# a different one on a per-connection basis using SELECT <dbid> where
# dbid is a number between 0 and 'databases'-1
databases 16

```

### 切换数据库 select number

```
127.0.0.1:6379> select 7
OK
127.0.0.1:6379[7]>

```

### 查看数据库数量 dbsize

```
127.0.0.1:6379[7]> DBSIZE
(integer) 0
127.0.0.1:6379[7]> select 0
OK
127.0.0.1:6379> DBSIZE
(integer) 5

```

### 查看数据库中所有的key  keys *

```
127.0.0.1:6379> keys * # 查看具体的key
1) "counter:__rand_int__"
2) "mylist"
3) "k1"
4) "myset:__rand_int__"
5) "key:__rand_int__"

```

### 情况当前数据库 flushdb

```
127.0.0.1:6379> DBSIZE
(integer) 5
127.0.0.1:6379> FLUSHDB
OK
127.0.0.1:6379> DBSIZE
(integer) 0

```

### 情况所有的数据库 flushall

