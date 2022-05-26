

## 阿里云服务器安全事件告警

近日，收到阿里云服务器的安全事件告警提示：


尊敬的阿里云用户育伢saas，您好！

您名下的资产 iZbp1hvca0d8vk8fgxk0b5Z 出现了安全告警：DDoS木马

建议您点击前往处理，防止黑客进一步行动

```

DDoS木马 紧急

发生时间：2022-05-07 14:24:52
IP：8.136.150.*** 172.18.52.***
告警描述：检测模型发现您的服务器上运行了DDoS木马，DDoS木马是用于从被攻陷主机上接受指令，对黑客指定目标发起DDoS攻击的恶意程序。

异常事件详情
文件路径：/usr/bin/lsof

恶意文件md5：e20c469e8010785a35e63407937bc20c

扫描来源方式： 进程启动扫描

检测方式：云查杀

进程id： 20984

进程命令行： lsof -i:50011

文件创建用户： N/A

文件修改时间： 2021-11-16 09:26:36

样本家族与特征：DDoS,BillGates

描述：黑客在入侵系统后植入的恶意程序，会占用您的带宽攻击其他服务器，同时可能影响自身业务的正常运行，危害较大。 此类恶意程序可能还存在自删除行为，或伪装成系统程序以躲避检测。如果发现该文件不存在，请检查是否存在可疑进程、定时任务或启动项。帮助文档：https://help.aliyun.com/knowledge_detail/36279.html

源文件下载：下载

事件说明

检测模型发现您的服务器上运行了DDoS木马，DDoS木马是用于从被攻陷主机上接受指令，对黑客指定目标发起DDoS攻击的恶意程序。

```

## ddos病毒

分布式拒绝服务攻击(英文意思是Distributed Denial of Service，简称DDoS)是指处于不同位置的多个攻击者同时向一个或数个目标发动攻击，或者一个攻击者控制了位于不同位置的多台机器并利用这些机器对受害者同时实施攻击。由于攻击的发出点是分布在不同地方的，这类攻击称为分布式拒绝服务攻击，其中的攻击者可以有多个。



## lsof程序

### lsof介绍

lsof，List Open Files 列出当前系统打开文件的工具。

在linux环境下，任何事物都以文件的形式存在，所以lsof通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。

### lsof安装和查看:

1，查看lsof所属的包

```
[root@blog ~]# whereis lsof
lsof: /usr/bin/lsof /usr/share/man/man1/lsof.1.gz

[root@blog ~]# rpm -qf /usr/bin/lsof
lsof-4.91-2.el8.x86_64

 ```

2,如果提示找不到lsof命令，可以用yum安装

```
[root@blog ~]# yum install lsof

``` 

3,查看版本

```
[root@blog ~]# lsof -v
lsof version information:
    revision: 4.91
```
 

4,查看帮助：

```
[root@blog ~]# lsof -h 

``` 

### lsof的应用例子：

1，查看系统中所有打开的文件

[root@blog ~]# lsof
 

2,查看某个用户打开的文件

 -u 参数用来指定要查看的用户

```
[root@blog ~]# lsof -u mysql

COMMAND     PID  USER   FD      TYPE             DEVICE   SIZE/OFF      NODE NAME
mysqld_sa 17246 mysql  cwd       DIR              253,1        129 201372497 /usr/local/soft/mysql
mysqld_sa 17246 mysql  rtd       DIR              253,1        272       128 /
mysqld_sa 17246 mysql  txt       REG              253,1    1219216  16999680 /usr/bin/bash 

```
 

3,查看有哪些进程正在打开某个文件？

```
[root@blog ~]# lsof /data/mysql/log/mysql-slow.log

COMMAND   PID  USER   FD   TYPE DEVICE SIZE/OFF     NODE NAME
mysqld  17700 mysql   29w   REG  253,1    11018 34357947 /data/mysql/log/mysql-slow.log
```

说明：lsof后加文件名即可列出正在打开文件的进程

 

4,列出某个进程正在打开的文件(最常用的用法)

 -p 指定要查看的进程

```
[root@blog ~]# lsof -p 17700

COMMAND   PID  USER   FD      TYPE             DEVICE   SIZE/OFF      NODE NAME
mysqld  17700 mysql  cwd       DIR              253,1       4096 302055050 /data/mysql/data
mysqld  17700 mysql  rtd       DIR              253,1        272       128 /
mysqld  17700 mysql  txt       REG              253,1 1078700088 218133053 /usr/local/soft/mysql/bin/mysqld
mysqld  17700 mysql  DEL       REG               0,17              3207767 /[aio]
mysqld  17700 mysql  DEL       REG               0,17              3207766 /[aio]
mysqld  17700 mysql  DEL       REG               0,17              3207765 /[aio]
mysqld  17700 mysql  DEL       REG               0,17              3207764 /[aio] 
```

 

输出字段的说明：

FD 表示文件描述符号：

如果值是3w,表示：它的文件描述符是 3 号，而 3 后面的 w ，表示以写的方式打开

TYPE 表示文件类型

NAME 表示文件路径

 

5，列出多个进程正在打开的文件(最常用的用法)

```
[root@blog ~]# lsof -p 7492,7493,7494 
``` 

6,列出所有的网络连接

 -i 用来查看网络连接的进程

```
[root@blog ~]# lsof -i


COMMAND     PID   USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
NetworkMa   834   root   18u  IPv4   21344      0t0  UDP blog:bootpc
nginx      7491   root   22u  IPv4 3087363      0t0  TCP *:http (LISTEN)
nginx      7491   root   23u  IPv4 3087364      0t0  TCP *:54321 (LISTEN)
nginx      7492  nginx   22u  IPv4 3087363      0t0  TCP *:http (LISTEN) 

```
 

7,指定的连接的类型:

列出所有tcp 网络连接信息

```
[root@blog ~]# lsof -i tcp
``` 

列出所有udp 网络连接信息

```
[root@blog ~]# lsof -i udp
``` 

8,列出在使用某个端口的进程

```
 -i :port用来指定要查看的端口
[root@blog ~]# lsof -i :3306
COMMAND   PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
mysqld  17700 mysql   31u  IPv6 3206926      0t0  TCP *:mysql (LISTEN)
```

9,列出nginx进程现在打开的文件

  -c 指定要查看的进程的名字
```
[root@blog ~]# lsof -c nginx

``` 

10,-n参数：不将IP转换为hostname，默认会进行转换,即默认不加上-n参数，

```
[root@blog ~]# lsof -n -i :3306
COMMAND   PID  USER   FD   TYPE  DEVICE SIZE/OFF NODE NAME
mysqld  17700 mysql   31u  IPv6 3206926      0t0  TCP *:mysql (LISTEN)
``` 

11,列出所有使用fd为指定值的进程

  -d:指定文件描述符的值

```
[root@blog ~]# lsof -d 1 
COMMAND     PID           USER   FD   TYPE             DEVICE SIZE/OFF     NODE NAME
systemd       1           root    1u   CHR                1,3      0t0    11236 /dev/null
systemd-j   513           root    1w   CHR                1,3      0t0    11236 /dev/null
```

## 病毒处理办法：删除该文件，并重新安装；

原来文件的大小和安装事件：

![](./assets/lsof_ddos.png)

执行  rm -f lsof

ls lsof

y

 