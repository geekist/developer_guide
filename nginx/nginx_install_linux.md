
## 一、二进制方式编译和安装

* 1、安装

在Ubuntu/Debian系

sudo apt-get install nginx

或者

RedHat/CentOS系

sudo yum install nginx 

* 2、卸载

通过yum安装的nginx，卸载时非常方便，只需要通过yum命令即可完成nginx的卸载。

卸载之前查看下nginx的安装情况
$ which nginx
/usr/sbin/nginx

通过yum命令删除nginx

***$ yum remove nginx***

## 二、源码编译和安装

### 2.1 安装Nginx依赖的库：

*  gcc-c++

gcc-c++(又叫做g++)是为gcc提供c++语言特性支持的

其实，就概念而言gcc是指整个gcc的这一套工具集合，它分为gcc前端和gcc后端（我个人理解为gcc外壳和gcc引擎），gcc前端对应各种特定语言（如c++/go等）的处理（对c++/go等特定语言进行对应的语法检查, 将c++/go等语言的代码转化为c代码等），gcc后端对应把前端的c代码转为跟你的电脑硬件相关的汇编或机器码等。（可能描述上不是特别准确，不过大体就是这个意思）

而就软件程序包而言，gcc.rpm就是那个gcc后端，而gcc-c++.rpm就是针对c++这个特定语言的gcc前端。这样的设计就保证了充分的灵活性，针对不同的程序语言，只需要开发不同的gcc前端就好了; 同时对于使用者来说，如果我只需要支持编译c++，那么我就只装一个c++的gcc前端就好了，就不会含有其他语言特性的程序代码，使得使用者的软件环境也比较轻量和干净。

***命令： yum install gcc-c++***

用来安装gcc和gcc-C++。 一般gcc已经安装好，如果安装好了，系统会忽略。

* zlib库

zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。

***命令： yum install -y zlib zlib-devel***


 devel包含普通包，只比普通包多了头文件。动态链接库的话两种包都有。编译的时候如果需要用到这个库，那么需要安装这个库的devel，因为需要头文件。

* prce

PCRE(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库。
注：pcre-devel是使用pcre开发的一个二次开发库，nginx也需要此库。

***命令：yum install -y pcre pcre-devel***

* 安装openssl

OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。

nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。

***命令：yum install -y openssl openssl-devel***

### 2.2 下载和安装Nginx的稳定版本

* 1、下载

在http://nginx.org/en/download.html下载nginx的稳定版本：
当前稳定版本是：1.20.1

* 2、上传到linux

通过ftp工具或者mobaxterm软件等上传到linux系统。

* 3、解压缩

进入压缩包所在的目录，解压该压缩包：
tar -zxvf nginx-1.20.1.tar.gz

解压后出现一个以nginx-1.20.1命名的目录

* 4、配置安装目录

在该目录下执行：
./configure --prefix=/usr/local/nginx --with-http_ssl_module
配置安装路径和ssl模块

* 5、编译

在该目录下执行：
***make***

c语言编译打包完成后，会在xxx/nginx-1.20.1/objs下生成一个nginx可执行程序，此时，在/usr/local下还没生成nginx安装目录


* 6、安装

在该目录下运行：

***make install***

 这一步骤之后，才是真正意义的完成了Nginx的安装，因为这一步，才会在刚才--prefix指定的安装路径/usr/local下，生成Nginx相关的目录。

## 三、Nginx的常用命令


* 1、 启动Nginx

```
默认方式启动
进入可执行程序目录下：
cd /usr/local/nginx/sbin
直接执行Nginx二进制程序：
./nginx
```

```
也可以使用命令，直接启动：
/usr/local/nginx/sbin/nginx（这种方式，默认使用/usr/local/nginx/conf/nginx.conf这个配置文件启动）
```

```
指定配置文件的方式启动
使用-c参数指定配置文件：
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```

* 2 停止Nginx

```
快速地停止服务
使用以下命令，快速停止Nginx服务：
/usr/local/nginx/sbin/nginx -s stop

使用-s stop可以强制停止Nginx服务，-s参数是向正在运行的Nginx服务发送signal信号量，Nginx程序通过nginx.pid文件，得到master主进程的进程id，再向运行中的master进程发送TERM信号来快速地关闭Nginx服务。

```

```
获取master id，使用命令：ps -ef | grep nginx
使用kill命令，强制关闭Nginx服务：
kill -TERM 主pid 或者kill -INT 主pid

可以通过kill命令直接向nginx master进程发送TERM或者INT信号，效果是一样的
```
```

优雅地停止服务
使用以下命令，优雅的关闭Nginx服务：
/usr/local/nginx/sbin/nginx -s quit 或者 kill -QUIT 主pid

如果希望Nginx服务可以正常地处理完当前所有请求之后再停止服务，那么可以使用这种优雅的方式停止服务。
```

* 3 刷新Nginx服务的配置

```
使运行中的Nginx服务刷新（重读）配置项并生效：
/usr/local/nginx/sbin/nginx -s reload

reload --重新加载，reload会重新加载配置文件，Nginx服务不会中断。而且reload时会测试conf语法等，如果出错会rollback用上一次正确配置文件保持正常运行。

总的来说就是：使用-s reload参数可以使运行中的Nginx服务重新加载nginx.conf配置文件，底层其实是先优雅关闭nginx服务，然后重新加载配置文件并启动
```


* 4 查看Nginx帮助文档

```
查看nginx的更多参数的使用，可以使用以下命令查看api帮助文档：
./nginx -h 或者 ./nginx -?

-h 或者 -? 参数会显示nginx命令支持的所有命令行参数
```

* 5 显示Nginx版本信息

```
使用-v参数显示Nginx的版本信息：
/usr/local/nginx/sbin/nginx -v

使用-V参数显示更多信息：
/usr/local/nginx/sbin/nginx -V
```


* 6 检查Nginx配置文件是否正确

```
使用以下命令：
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf -t

测试配置选项时，使用-q参数不把error级别以下的信息输出到屏幕：
/usr/local/nginx/sbin/nginx -t -q
```

* 7 日志文件回滚

```
使用以下命令进行日志的重新生成：
/usr/local/nginx/sbin/nginx -s reopen

使用-s reopen参数可以重新打开日志文件，再重新打开时就会生成新的日志文件，这样使得日志文件不至于过大，我们公司的Nginx服务，nginx日志没有切分，导致日志文件就超级大，不利于排查问题。

注意：操作前需要先把当前日志文件改名或转移到其他目录中进行备份，然后再进行日志的重新生成
```

## 四、Nginx的卸载

* 1.查看nginx运行情况

***ps -ef |grep nginx***

* 2.停止Nginx

***nginx -s stop***

* 3.查看nginx的安装情况

***nginx -V***

* 4. 删除Nginx的自动启动。

***chkconfig nginx off***


* 5.从源头删除Nginx。

```
[root@idroot.net ~]# rm -rf /usr/sbin/nginx

[root@idroot.net ~]# rm -rf /etc/nginx

[root@idroot.net ~]# rm -rf /etc/init.d/nginx

```

* 6.检查nginx的残留

***whereis nginx
***