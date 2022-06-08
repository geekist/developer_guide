
# 一、Nginx介绍

## Nginx是什么

Nginx是一种WEB服务器，它基于REST架构风格，以统一资源描述符(Uniform Resources Identifier)URI或者统一资源定位符(Uniform Resources Locator)URL作为沟通依据，通过HTTP协议提供各种网络服务。

Nginx是一款自由的、开源的、高性能的HTTP服务器和反向代理服务器；同时也是一个IMAP、POP3、SMTP代理服务器；Nginx可以作为一个HTTP服务器进行网站的发布处理，另外Nginx可以作为反向代理进行负载均衡的实现。

Nginx使用基于事件驱动架构，使得其可以支持数以百万级别的TCP连接。

Nginx是一个跨平台服务器，可以运行在Linux，Windows，FreeBSD，Solaris，AIX，Mac OS等操作系统上。

对于Nginx的初学者可能不太容易理解web服务器究竟能做什么，特别是之前用过Apache服务器的，以为Nginx可以直接处理php、java，实际上并不能。对于大多数使用者来说，Nginx只是一个静态文件服务器或者http请求转发器，它可以把静态文件的请求直接返回静态文件资源，把动态文件的请求转发给后台的处理程序，例如php-fpm、apache、tomcat、jetty等，这些后台服务，即使没有nginx的情况下也是可以直接访问的（有些时候这些服务器是放在防火墙的面，不是直接对外暴露，通过nginx做了转换）。

## Nginx的架构
![Nginx架构](https://github.com/geekist/developer_guide/blob/main/nginx/assets/nginx架构.jpg)

## Nginx的特性

### Nginx的基本特性

- 可针对静态资源高速高并发访问及缓存。

- 可使用反向代理加速，并且可进行数据缓存。

- 具有简单负载均衡、节点健康检查和容错功能。

- 支持远程 FastCGI 服务的缓存加速。
 
- 支持 FastCGI、Uwsgi、SCGI、Memcached Servers 的加速和缓存。

- 支持SSL、TLS、SNI。
 
- 具有模块化的架构：过滤器包括 gzip 压缩、ranges 支持、chunked 响应、XSLT、SSI 及图像缩放等功能。在SSI 过滤中，一个包含多个 SSI 的页面，如果经由 FastCGI 或反向代理，可被并行处理。


### Nginx Web的服务特性

- 支持基于名字、端口及IP的多虚拟主机站点。

- 支持 Keep-alive 和 pipelined 连接。

- 可进行简单、方便、灵活的配置和管理。

- 支持修改 Nginx 配置，并且在代码上线时，可平滑重启，不中断业务访问。

- 可自定义访问日志格式，临时缓冲写日志操作，快速日志轮询及通过 rsyslog 处理日志。

- 可利用信号控制 Nginx 进程。

- 支持 3xx-5xx HTTP状态码重定向。

- 支持 rewrite 模块，支持 URI 重写及正则表达式匹配。

- 支持基于客户端 IP 地址和 HTTP 基本认证的访问控制。

- 支持 PUT、DELETE、MKCOL、COPY 及 MOVE 等特殊的 HTTP 请求方法。

- 支持 FLV 流和 MP4 流技术产品应用。

- 支持 HTTP 响应速率限制。

- 支持同一 IP 地址的并发连接或请求数限制。

- 支持邮件服务代理。

## Nginx反向代理

- 一、反向代理

**正向代理**

正向代理（forward）意思是一个位于客户端和原始服务器 (origin server) 之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标 (原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

正向代理是为我们服务的，即为客户端服务的，客户端可以根据正向代理访问到它本身无法访问到的服务器资源。

正向代理对我们是透明的，对服务端是非透明的，即服务端并不知道自己收到的是来自代理的访问还是来自真实客户端的访问。



**反向代理**

反向代理（Reverse Proxy）方式是指以代理服务器来接受 internet 上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给 internet 上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

反向代理是为服务端服务的，反向代理可以帮助服务器接收来自客户端的请求，帮助服务器做请求转发，负载均衡等。

反向代理对服务端是透明的，对我们是非透明的，即我们并不知道自己访问的是代理服务器，而服务器知道反向代理在为他服务。

![正向代理](https://github.com/geekist/developer_guide/blob/main/nginx/assets/delegate.png)

## Nginx负载均衡

如果请求数过大，单个服务器解决不了，我们增加服务器的数量，然后将请求分发到各个服务器上，将原先请求集中到单个服务器的情况改为请求分发到多个服务器上，就是负载均衡。

Nginx支持的负载均衡调度算法方式如下：


weight轮询(默认，常用)：接收到的请求按照权重分配到不同的后端服务器，即使在使用过程中，某一台后端服务器宕机，Nginx会自动将该服务器剔除出队列，请求受理情况不会受到任何影响。这种方式下，可以给不同的后端服务器设置一个权重值(weight)，用于调整不同的服务器上请求的分配率；权重数据越大，被分配到请求的几率越大；该权重值，主要是针对实际工作环境中不同的后端服务器硬件配置进行调整的。ip_hash（常用）：每个请求按照发起客户端的ip的hash结果进行匹配，这样的算法下一个固定ip地址的客户端总会访问到同一个后端服务器，这也在一定程度上解决了集群部署环境下session共享的问题。


fair：智能调整调度算法，动态的根据后端服务器的请求处理到响应的时间进行均衡分配，响应时间短处理效率高的服务器分配到请求的概率高，响应时间长处理效率低的服务器分配到的请求少；结合了前两者的优点的一种调度算法。但是需要注意的是Nginx默认不支持fair算法，如果要使用这种调度算法，请安装upstream_fair模块。url_hash：按照访问的url的hash结果分配请求，每个请求的url会指向后端固定的某个服务器，可以在Nginx作为静态服务器的情况下提高缓存效率。同样要注意Nginx默认不支持这种调度算法，要使用的话需要安装Nginx的hash软件包。

## Nginx的工作模式

启动Nginx后，其实就是在80端口启动了Socket服务进行监听，如图所示，Nginx涉及Master进程和Worker进程。

Master进程的作用是读取并验证配置文件nginx.conf；管理worker进程；

Worker进程的作用是：每一个Worker进程都维护一个线程（避免线程切换），处理连接和请求；注意Worker进程的个数由配置文件决定，一般和CPU个数相关（有利于进程切换），配置几个就有几个Worker进程。

![工作进程](https://github.com/geekist/developer_guide/blob/main/nginx/assets/worker.jpg)

## Nginx热部署

所谓热部署，就是配置文件nginx.conf修改后，不需要stop Nginx，不需要中断请求，就能让配置文件生效！

不运行enginx，检查配置文件是否正确：

```
nginx -t
```

不停止nginx服务，重新加载nginx

```
nginx -s reload
```

## Nginx学习资源

* 官方下载网站：http://nginx.org/

* 官网： https://www.nginx.com/

* 中文文档：  https://www.nginx.cn/doc/　

* Nginx书籍： http://nginx.org/en/books.html

# 二、Nginx的安装和卸载


可以在Nginx官网：http://nginx.org/en/download.html 下载nginx，最新版本是1.20.2

网页上提供了Nginx 服务器三种版本的下载，分别是：

* 开发版（Mainline version）

* 稳定版本（Stable version

* 过期版本（Legacy versions）

页面上下载部分各链接的具体含义：

“CHANGES-x.xx”链接，记录的是对应版本的功能变更日志。包括新增功能、功能的优化和功能缺陷的修复等。

“nginx-x.x.x”链接，是 Nginx服务器的 Linux版本下载地址。


“pgp”链接，记录的是提供下载的版本使用PGP加密自由软件GnuPG计算后的签名。PGP可以理解为Pretty Good Privacy。这些数据可以用于下载文件的验证。

“nginx/Windows-x.x.x”链接，是 Nginx 服务器的Windows版本下载地址。


## window环境下Nginx的安装和卸载

nginx在windows下免安装，直接运行即可

## linux环境下Nginx的安装和卸载

### 彻底卸载nginx

* step1：查看是否有正在运行的nginx进程,如果有，则停滞nginx

```
ps -ef|grep nginx

```
停止nginx 服务,在nginx的目录下执行

```
./nginx -s stop
```

* step2:查找nginx的所有相关目录

```
which nginx
```

```
[root@localhost ~]# whereis nginx
nginx: /usr/local/nginx
```

```
[root@localhost ~]# find / -name nginx
/var/spool/mail/nginx
/usr/local/nginx
/usr/local/nginx/sbin/nginx

```

* step3:对找到的目录删除相关文件

```
[root@localhost ~]# rm -rf /usr/local/nginx
[root@localhost ~]# rm -rf /usr/local/nginx/sbin/nginx
[root@localhost ~]# rm -rf /var/spool/mail/nginx

```

* step4:使用yum清理

```
[root@localhost ~]# yum remove nginx
Loaded plugins: fastestmirror
No Match for argument: nginx
No Packages marked for removal
```

### 使用yum命令从云应用仓库下载并安装nginx

```
$ sudo yum -y install nginx   # 安装 nginx
```

如果没有配置EPEL仓库，可以先运行下面的命令来完成安装：
```
sudo yum install epel-release

```

安装成功后，默认的网站目录为： /usr/share/nginx/html

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

此时，nginx已经被添加到了环境变量中，可以直接在任意路径下使用。

### 从



# 三、启动和停止nginx

# 四、Nginx配置文件介绍

# 无、websocket ssl、gzip等

## [Linux环境下Nginx安装和配置](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_install_linux.md)

## [Windows环境下Nginx安装和配置](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_install.md)

## [nginx配置文件详解](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_config.md)

## [nginx使用gzip压缩](./nginx_gzip.md)

## [YuYa_Nginx配置](https://github.com/geekist/developer_guide/blob/main/nginx/yuya_nginx_conf.md)

## [nginx常用命令和疑难问题解决](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_command.md)

