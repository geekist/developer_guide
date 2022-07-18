- [一、Nginx介绍](#一nginx介绍)
  - [Nginx是什么](#nginx是什么)
  - [Nginx的架构](#nginx的架构)
  - [Nginx的特性](#nginx的特性)
    - [Nginx的基本特性](#nginx的基本特性)
    - [Nginx Web的服务特性](#nginx-web的服务特性)
  - [Nginx反向代理](#nginx反向代理)
  - [Nginx负载均衡](#nginx负载均衡)
  - [Nginx的工作模式](#nginx的工作模式)
  - [Nginx热部署](#nginx热部署)
  - [Nginx学习资源](#nginx学习资源)
- [二、Nginx的安装和卸载](#二nginx的安装和卸载)
  - [window环境下Nginx的安装和卸载](#window环境下nginx的安装和卸载)
  - [linux环境下Nginx的安装和卸载](#linux环境下nginx的安装和卸载)
    - [彻底卸载nginx](#彻底卸载nginx)
    - [使用yum命令从云应用仓库下载并安装nginx](#使用yum命令从云应用仓库下载并安装nginx)
    - [从源代码编译安装nginx](#从源代码编译安装nginx)
- [三、启动和停止nginx](#三启动和停止nginx)
  - [启动nginx](#启动nginx)
  - [带配置文件的启动](#带配置文件的启动)
  - [停止nginx](#停止nginx)
  - [安全停止nginx](#安全停止nginx)
  - [热启动（修改配置文件后重新启动）](#热启动修改配置文件后重新启动)
- [四、Nginx配置文件介绍](#四nginx配置文件介绍)
  - [nginx配置文件语法：](#nginx配置文件语法)
  - [nginx 基本模块分类](#nginx-基本模块分类)
  - [全局块配置](#全局块配置)
    - [event模块](#event模块)
    - [http模块](#http模块)
    - [server模块](#server模块)
      - [listen指令](#listen指令)
      - [server_name:](#server_name)
    - [Location 模块](#location-模块)
      - [location功能](#location功能)
      - [localtion语法](#localtion语法)
      - [location匹配顺序](#location匹配顺序)
      - [root命令index命令alias和return命令](#root命令index命令alias和return命令)
      - [return指令](#return指令)
      - [proxy_pass](#proxy_pass)
  - [error_page配置命令](#error_page配置命令)
  - [try_files指令](#try_files指令)
- [五、Nginx日志配置](#五nginx日志配置)
  - [Nginx的访问日志](#nginx的访问日志)
  - [Nginx的错误日志](#nginx的错误日志)
- [六、其他配置、websocket ssl、gzip等](#六其他配置websocket-sslgzip等)




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

### 从源代码编译安装nginx

* step1：首先从nginx官网下载压缩包，nginx-1.22.0.tar.gz，然后使用ftp工具等，将nginx上传到linux。

```
cd /usr/local

mkdir nginx

cd nginx
```

* step2：安装GCC与dev库

```
GCC编译器:yum install gcc gcc-c++

正则表达式PCRE库:yum install -y pcre pcre-devel

zlib压缩库:yum install -y zlib zlib-devel

OpenSSL开发库:yum install -y openssl openssl-devel
```

可以一键安装：
```
yum -y install gcc zlib zlib-devel pcre pcre-devel openssl openssl-devel

```

* step3:编译Nginx

```
//进入nginx目录
cd /usr/local/nginx
//进入目录
cd nginx-1.13.7
//执行命令
./configure
//执行make命令
make
//执行make install命令
make install
```
至此，nginx安装成功

# 三、启动和停止nginx

## 启动nginx

```
执行启动命令：
./nginx                       //启动
```

## 带配置文件的启动


```
./nginx -c conf/nginx.conf
```

## 停止nginx


```
./nginx -s stop
```

## 安全停止nginx


```
./nginx -s quit                
```

## 热启动（修改配置文件后重新启动）

```
./nginx -s reload 
```
完整的过程如下：

```
cd /usr/local/nginx/sbin/     //进入目录

执行启动命令：
./nginx                       //启动

带参数的启动命令：

./nginx -c conf/nginx.conf

停止nginx
./nginx -s stop               //停止

安全退出nginx
./nginx -s quit               //安全退出

nginx热启动，修改了配置文件后，可以使用此方法不停止nginx
./nginx -s reload            //重载配置文件（修改了配置文件需要执行此命令 比较常用）
```

# 四、Nginx配置文件介绍

## nginx配置文件语法：

* 组成：


配置文件由注释行，指令块配置项和一系列指令配置项组成。
```
# user nobody;
worker_processes 4;
events {
    worker_connections 1024;
}
```

注释行：

字符"#"表示该行是注释行，nginx在读取配置文件时将忽略的文本。

块配置项：

块配置项由一个块配置项名和一对大括号组成。

块配置后面是否带有参数，如location /webstatic {}，取决于解析该配置块的模块。

指令配置项：

每一条指令由配置项名称和值参数组成，值参数可以时一个或多个附加参数，取决于解析该条指令的模块。

指令配置项总是以分号结尾(;)。

* 指令特点

指令都有作用域，如上面例子，worker_connections 只能放在events区块才有意义。

配置块能相互嵌套。在某些情况下不同配置块能够相互嵌套。如在http区段，可以声明一个或多个server区段，server区段又可以插入一个或多个location区段。

最后同样重要的是配置的继承。在一个区段中嵌套其他区段，那么被嵌套的区段会继承其父区段的配置。在嵌套模块中重新声明指令会覆盖该继承。

## nginx 基本模块分类

1、全局块：配置影响nginx全局的指令。一般有运行nginx服务器的用户组，nginx进程pid存放路径，日志存放路径，配置文件引入，允许生成worker process数等。

2、events块：配置影响nginx服务器或与用户的网络连接。有每个进程的最大连接数，选取哪种事件驱动模型处理连接请求，是否允许同时接受多个网路连接，开启多个网络连接序列化等。

3、http块：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。如文件引入，mime-type定义，日志自定义，是否使用sendfile传输文件，连接超时时间，单连接请求数等。

4、server块：配置虚拟主机的相关参数，一个http中可以有多个server。

5、location块：配置请求的路由，以及各种页面的处理情况。

## 全局块配置

全局配置主要配置nginx在运行时与具体业务功能（比如http服务或者email服务代理）无关的一些参数，比如工作进程数，运行的身份等。

```
user  www ;

#指定nginx进程使用什么用户启动

worker_processes 4;

#指定启动多少进程来处理请求，一般情况下设置成CPU的核数，如果开启了ssl和gzip更应该设置成与逻辑CPU数量一样甚至为2倍，可以减少I/O操作。使用grep ^processor /proc/cpuinfo | wc -l查看CPU核数。



worker_cpu_affinity 0001 0010 0100 1000; 

#worker_cpu_affinity 0001 0010 0100 1000;: 在高并发情况下，通过设置将CPU和具体的进程绑定来降低由于多核CPU切换造成的寄存器等现场重建带来的性能损耗。如



error_log  /data/logs/nginx_error.log  crit;

#error_log /data/logs/nginx_error.log crit;: error_log是个主模块指令，用来定义全局错误日志文件。日志输出级别有debug、info、notice、warn、error、crit可供选择，其中，debug输出日志最为最详细，而crit输出日志最少。




pid /usr/local/webserver/nginx/nginx.pid;

pid指定进程pid文件的位置。

worker_rlimit_nofile 65535;

worker_rlimit_nofile 65535;用于指定一个nginx进程可以打开的最多文件描述符数目，这里是65535，需要使用命令“ulimit -n 65535”来设置。
```

**其中比较重要的是工作进程数：如果是4核CPU的话，要换成4**


### event模块

events模块主要配置影响nginx服务器或与用户的网络连接

```
 events{
  use epoll;
  worker_connections      65536;
}
```

use epoll;

use是个事件模块指令，用来指定Nginx的工作模式。Nginx支持的工作模式有select、poll、kqueue、epoll、rtsig和/dev/poll。其中select和poll都是标准的工作模式，kqueue和epoll是高效的工作模式，不同的是epoll用在Linux平台上，而kqueue用在BSD系统中。
对于Linux系统，epoll工作模式是首选。在操作系统不支持这些高效模型时才使用select。

worker_connections 65536;

每一个worker进程能并发处理（发起）的最大连接数（包含与客户端或后端被代理服务器间等所有连接数）。

nginx作为反向代理服务器，计算公式 最大连接数 = worker_processes * worker_connections/4，所以这里客户端最大连接数是65536，这个可以增到到8192都没关系，看情况而定，但不能超过后面的worker_rlimit_nofile。

当nginx作为http服务器时，计算公式里面是除以2。进程的最大连接数受Linux系统进程的最大打开文件数限制，在执行操作系统命令ulimit -n 65536后worker_connections的设置才能生效。


### http模块

可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。

```
http{
  include       mime.types;
  default_type  application/octet-stream;
  #charset  gb2312;
 }
```

主要配置参数：

```
include mime.types;

include 是个主模块指令，实现对配置文件所包含的文件的设定，可以减少主配置文件的复杂度。类似于Apache中的include方法。

default_type  application/octet-stream;

default_type属于HTTP核心模块指令，这里设定默认类型为二进制流，也就是当文件类型未定义时使用这种方式，例如在没有配置PHP环境时，Nginx是不予解析的，此时，用浏览器访问PHP文件就会出现下载窗口。

charset gb2312; 

指定客户端编码格式。

server_names_hash_bucket_size 128;

服务器名字的hash表大小。

client_header_buffer_size 32k;

用来指定来自客户端请求头的header buffer 大小。对于大多数请求，1K的缓存已经足够了，如果自定义了消息头或有更大的cookie，可以增大缓存区大小。

large_client_header_buffers 4 128k;

用来指定客户端请求中较大的消息头的缓存最大数量和大小，4为个数，128k为大小，最大缓存为4个128KB。

client_max_body_size 8m;

客户端请求的最大的单个文件字节数。

client_max_body_size 10m;

允许客户端请求的最大单文件字节数。如果有上传较大文件，请设置它的限制值。

client_body_buffer_size 128k;

缓冲区代理缓冲用户端请求的最大字节数。

sendfile on ;

开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，减少用户空间到内核空间的上下文切换。对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。开启 tcp_nopush on; 和tcp_nodelay on; 防止网络阻塞。

keepalive_timeout 65 :

长连接超时时间，单位是秒，这个参数很敏感，涉及浏览器的种类、后端服务器的超时设置、操作系统的设置，可以另外起一片文章了。长连接请求大量小文件的时候，可以减少重建连接的开销，但假如有大文件上传，65s内没上传完成会导致失败。如果设置时间过长，用户又多，长时间保持连接会占用大量资源。

client_body_timeout 60s;

用于设置客户端请求主体读取超时时间，默认是60s。如果超过这个时间，客户端还没有发送任何数据，nginx将返回Request time out(408)错误。

send_timeout :

用于指定响应客户端的超时时间。这个超时仅限于两个连接活动之间的时间，如果超过这个时间，客户端没有任何活动，Nginx将会关闭连接。

gzip on;

开启gzip压缩输出

gzip_min_length 1k; 

最小压缩文件大小，页面字节数从header头的Content-Length中获取。默认值为0，不管多大页面都压缩，建议设置成大于1K的字节数，小于1K可能会越压越大。

gzip_buffers 4 16k; 

压缩缓冲区，表示申请四个16K的内存作为压缩结果流缓存，默认是申请与原始数据大小相同的内存空间来存储gzip压缩结果。

gzip_http_version 1.1;

 用于设置识别HTTP协议版本，默认是1.1，目前主流浏览器都已成指出。（默认1.1，前端如果是squid2.5请使用1.0）

gzip_comp_level 6; 压缩等级，1压缩比最小，处理速度最快，9压缩比最大，传输速度快，但是消耗CPU资源。

```

### server模块

http服务上支持若干虚拟主机。每个虚拟主机一个对应的server配置项，配置项里面包含该虚拟主机相关的配置。每个server通过监听地址或端口来区分。

在提供mail服务的代理时，也可以建立若干server。

通常server指令中会包含一条listen指令，用于指定该虚拟服务器将要监听的IP地址和端口。示例如下：


```
server {
    listen      80;
    server_name example.org www.example.org;
    ...
}
```

#### listen指令

* 如果不填写端口，则采用标准端口。

* 如果不填写ip地址，则监听所有地址。

* 如果缺少整条listen指令，则标准端口是80/tcp，默认端口是8000/tcp，由超级用户的权限决定。

* 如果有多个server配置了相同的ip地址和端口，Nginx会匹配server_name指令与请求头部的host字段。

#### server_name:

* server_name指令的参数可以是精确的文本、通配符或正则表达式。通配符可以在字符串的头部、尾部或两端包含*，*可以匹配任意字符。Nginx采用Perl格式的正则表达式，以~开头。以下是一个精确匹配的例子：

* 如果有多个server_name匹配host字段，Nginx根据以下规则选择第一个相匹配的server处理请求：
```
精确匹配
以 * 开始的最长通配符，如 *.example.org
以 * 结尾的最长通配符，如 mail. *
第一个匹配的正则表达式（根据在配置文件中出现的先后顺序）
```
如果找不到任何与host字段相匹配的server_name，Nginx会根据请求端口将其发送给默认的server。默认server就是配置文件中第一个出现的server，也可以通过default_server指定某个server为默认server

nginx支持三种类型的 虚拟主机配置

* 1、基于ip的虚拟主机, (一个主机绑定多个ip地址)

```
server{
  listen       192.168.1.1:80;
  server_name  localhost;
}

server{
  listen       192.168.1.2:80;
  server_name  localhost;
}

```

* 2、基于域名的虚拟主机(servername)

```
#域名可以有多个，用空格隔开
server{
  listen       80;
  server_name  www.nginx1.com www.nginx2.com;
}

server{
  listen       80;
  server_name  www.nginx3.com;
}

``` 

* 3、基于端口的虚拟主机(listen不写ip的端口模式)

```
server{
  listen       80;
  server_name  localhost;
}

server{
  listen       81;
  server_name  localhost;
}

```

### Location 模块

#### location功能

location是Nginx中的块级指令(block directive),location指令的功能是用来匹配不同的url请求，进而对请求做不同的处理和响应。nginx用请求URI与location中配置的URI做匹配。

例如：

```
http { 
  server {
      listen 80;
    	server_name www.yayujs.com;
    	location / {
      	root /home/www/ts/;
	      index index.html;
    	}
  }
}
```

大致的意思是，当你访问 www.yayujs.com 的 80 端口的时候，返回/home/www/ts/index.html 文件。

#### localtion语法

location有两种匹配规则：

匹配URL类型，有四种参数可选，当然也可以不带参数。

```
location [ = | ~ | ~* | ^~ ] uri { … }
```

命名location，用@标识，类似于定于goto语句块。

```
location @name { … }
```

location匹配参数解释：

（1） “=” ，匹配后面的字符串，要求路径完全匹配

```
server {
    listen  8099;

    location = /123/ {
          [......]
          }
}

要求：匹配"/123/",要求在路径字段完全匹配；

输入：http://website.com/123匹配，大部分浏览器会默认加斜杠；
输入：http://website.com/123/?param1&param2匹配，忽略 querystring
输入：http://website.com/1234/不匹配
```

（2） “~”，匹配后面的正则匹配表达式，区分大小写。

```
location ~ /Abc/ {
  .....
}

要求： 在路径字典完全匹配“/Abc”

#http://abc.com/Abc/ [匹配成功]
#http://abc.com/abc/ [匹配失败]
```

（3）“~*”，匹配后面的正则表达式，并忽略大小写

```
location ~* /Abc/ {
  .....
}

# 则会忽略 uri 部分的大小写
#http://abc.com/Abc/ [匹配成功]
#http://abc.com/abc/ [匹配成功]
```

（4）“^~”，前缀匹配，匹配之后的正则表达式开头的字符串，如果命中，则其他的location都不执行匹配。

```
location ^~ /index/ {
  .....
}
#以 /index/ 开头的请求，都会匹配上
#http://abc.com/index/index.page  [匹配成功]
#http://abc.com/error/error.page [匹配失败]
```

（5）不加任何规则时，默认是大小写敏感，前缀匹配，相当于加了“~”与“^~”

```
location /index/ {
  ......
}
#http://abc.com/index  [匹配成功]
#http://abc.com/index/index.page  [匹配成功]
#http://abc.com/test/index  [匹配失败]
#http://abc.com/Index  [匹配失败]
```

（6）“@”，nginx内部跳转

```
location /index/ {
  error_page 404 @index_error;
}

location @index_error {
  .....
}
#以 /index/ 开头的请求，如果链接的状态为 404。则会匹配到 @index_error 这条规则上。
```

#### location匹配顺序

```
= > ^~ > ~ | ~* > 最长前缀匹配 > /
```

解释：

* 序号越小优先级越高
 
* 最高优先级 location =    # 精准匹配


* 第二优先级 location ^~   # 带参前缀匹配


* 第三优先级 location ~    # 正则匹配（区分大小写）


* 第四优先级 location ~*   # 正则匹配（不区分大小写）


* 第五优先级 location /a   # 普通前缀匹配，优先级低于带参数前缀匹配。

* 第六优先级 location /    # 任何没有匹配成功的，都会匹配这里处理

例子：

```
location = /  {
#规则A
}

location = /login {
#规则B
}

location ^~ /static/ {
#规则C
}

location ~ \.(gif|jpg|png|js|css)$ {
#规则D
}

location ~* \.png$ {
#规则E
}

location !~ \.xhtml$ {
#规则F
}

location !~* \.xhtml$ {
#规则G
}

location / {
#规则H
}

```

假设：

```
访问根目录/， 比如http://localhost/ 将匹配规则A

访问 http://localhost/login 将匹配规则B，http://localhost/register 则匹配规则H

访问 http://localhost/static/a.html 将匹配规则C

访问 http://localhost/b.jpg 将匹配规则D和规则E，但是规则D顺序优先，规则E不起作用， 而 http://localhost/static/c.png 则优先匹配到 规则C

访问 http://localhost/a.PNG 则匹配规则E， 而不会匹配规则D，因为规则E不区分大小写。

访问 http://localhost/a.xhtml 不会匹配规则F和规则G，http://localhost/a.XHTML不会匹配规则G，因为不区分大小写。规则F，规则G属于排除法，符合匹配规则但是不会匹配到。

访问 http://localhost/qll/id/1111 则最终匹配到规则H，因为以上规则都不匹配。
```

#### root命令index命令alias和return命令

* root

root指令用于指定网站的根目录。

* index

index 指令用于设置网站的默认首页。

除了用在location，index还可以使用在http、server块中。

* alias

root和alias都可以用来定义网站的根目录，但alias不会追加location匹配到的部分，

例如：

```
 location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
 }

当nginx接收到：nginx主机IP/helloworld/hello.html时，

首先根据location的匹配规则，大小写敏感的前缀匹配方式，"/" 匹配上后，匹配剩余的部分"helloworld/hello.html"会追加到root的后面，通过root + location 的方式来返回：

nginx主机IP/usr/share/nginx/html/helloworld/hello.html

当nginx收到nginx主机IP/时，

采用root + index 的方式，nginx主机IP/usr/share/nginx/html/index.html
```

使用root和alias的区别：

```
#不基于正则
location / {
    #root /usr/share/nginx/html;
    alias /usr/share/nginx/html;
    index index.html index.htm;
}

#基于正则
location ~ /helloworld/.*\.png$
    #root /usr/share/nginx/html/helloworld;
    alias /usr/share/nginx/html/helloworld;
    index index.html;
}

不基于正则的时候，使用http://192.168.48.131/test/a.png来测试：

使用root时，访问的资源路径是/usr/share/nginx/html/test/a.png，会把匹配到的/和test/a.png追加到root指定的路径之后。
使用alias时，访问的资源路径是/usr/share/nginx/htmltest/a.png，你会看到它并没有携带上匹配的/，而是把test/a.png追加到了alias路径之后。


在location是正则表达式的时候，使用http://192.168.48.131/helloworld/helloworld/a.png来测试：

使用root时，访问的资源路径是/usr/share/nginx/html/helloworld/helloworld/a.png，会把pattern部分直接追加到root指定的路径之后。
使用alias时，访问的资源路径是/usr/share/nginx/html/helloworld，没错，就是alias的绝对路径，它不会把正则表达式部分追加到alias指定的路径之后。
```

#### return指令

使用return可以返回数据或资源文件的路径

语法格式如下：

```
return 响应码 [响应的数据]

return 资源文件路径

当return只有响应码的时候，如果抛出的响应码在error_page中有，那么会返回error_page的响应，比如return 500;

```

#### proxy_pass

proxy_pass用于设置被代理服务器的地址，可以是主机名称（https://www.baidu.com这样的）、IP地址(域名加端口号)的形式。



假设请求：http://localhost/online/wxapi/test/loginSwitch

* 第一种情况：proxy_pass结尾有`IP+port+/`  ---有杠去除location

```
location /online/wxapi/ {
        proxy_pass http://localhost:8080/;
        proxy_set_header X-Real-IP $remote_addr;
}
```
proxy_pass后面有斜杠，则拼接不包含前缀：

代理后的实际地址：http://localhost:8080/test/loginSwitch

* 第二种情况: proxy_pass最后有：`IP_port`（不带杠)---保留location
```
location /online/wxapi/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
}
```
IP+Port：拼接包含前缀

代理后的实际地址：http://localhost:8080/online/wxapi/test/loginSwitch

* 第三种情况:proxy_pass最后有/web ---去除location，剩余部分拼接

```
location /online/wxapi/ {
        proxy_pass http://localhost:8080/web;
        proxy_set_header X-Real-IP $remote_addr;
}
```

IP+Port+/+web，最后没有斜杠，拼接不包含前缀，剩余部分，可能会单词拼接；

代理后的实际地址：http://localhost:8080/webtest/loginSwitch

注意：因为是拼接剩余部分，所以路径中可能有单个词的拼接，比如webtest

* 第四种情况 proxy_pass最后有/web/

location /online/wxapi/ {
        proxy_pass http://localhost:8080/web/;
        proxy_set_header X-Real-IP $remote_addr;
}

IP+Port+/+web + /，最后有斜杠，拼接不包含前缀

代理后的实际地址：http://localhost:8080/web/test/loginSwitch

但是，如果location是正则表达式，则上述不正确，参见yuya的config

## error_page配置命令

语法：

```
error_page code [ code... ] [ = | =answer-code ] uri | @named_location 
```

默认值：

no 

使用字段：

```
http, server, location, location 中的if字段 
```

实例:

* nginx指令error_page的作用是当发生错误的时候能够显示一个预定义的uri

```
error_page 502 503 /50x.html;
location = /50x.html {
    root /usr/share/nginx/html;
}  
``` 

当访问出现502、503的时候就能返回50x.html中的内容，这样实际上产生了一个内部跳转(internal redirect)，这里需要注意是否可以找到50x.html页面，所以加了个location保证找到你自定义的50x页面。

* 自己定义这种情况下的返回状态吗
```
error_page 502 503 =200 /50x.html;
location = /50x.html {
    root /usr/share/nginx/html;
}  
``` 
这样用户访问产生502 、503的时候给用户的返回状态是200，内容是50x.html。

*动态返回内容

当error_page后面跟的不是一个静态的内容的话，比如是由proxyed server或者FastCGI/uwsgi/SCGI server处理的话，server返回的状态(200, 302, 401 或者 404）也能返回给用户。
```
error_page 404 = /404.php;
location ~ \.php$ {
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
} 
```

* 别名

设置一个named location，然后在里边做对应的处理。
```
error_page 500 502 503 504 @jump_to_error;
location @jump_to_error {    
    proxy_pass http://backend;
}
```

同时也能够通过使客户端进行302、301等重定向的方式处理错误页面，默认状态码为302。

error_page 403      http://example.com/forbidden.html;
error_page 404 =301 http://example.com/notfound.html;


* Nginx 自定义404错误页面配置中有无等号的区别

```
error_page 404 /404.html 可显示自定义404页面内容，正常返回404状态码。
error_page 404 =200 /404.html 可显示自定义404页面内容，但返回200状态码。
error_page 404 /404.php 如果是动态404错误页面，包含 header 代码（例如301跳转），将无法正常执行。正常返回404代码。
error_page 404 = /404.php 如果是动态404错误页面，包含 header 代码（例如301跳转），加等号配置可以正常执行，返回php中定义的状态码。但如果php中定义返回404状态码，404状态码可以正常返回，但无法显示自定义页面内容（出现系统默认404页面），这种情况可以考虑用410代码替代（ header("HTTP/1.1 410 Gone"); 正常返回410状态码，且可正常显示自定义内容）。
```

## try_files指令

try_files是nginx中http_core核心模块所带的指令，主要是能替代一些rewrite的指令，提高解析效率。

官网的文档为http://nginx.org/en/docs/http/ngx_http_core_module.html#try_files

语法：
```
语法：try_files file ... uri；
try_files file ... = code；

```
范围：
server，location

try_files的语法解释：

Checks the existence of files in the specified order and uses the first found file for request processing; the processing is performed in the current context. The path to a file is constructed from the *file*parameter according to the root and alias directives. It is possible to check directory’s existence by specifying a slash at the end of a name, e.g. “$uri/”. If none of the files were found, an internal redirect to the *uri* specified in the last parameter is made.

关键点1：按指定的file顺序查找存在的文件，并使用第一个找到的文件进行请求处理

关键点2：查找路径是按照给定的root或alias为根路径来查找的

关键点3：如果给出的file都没有匹配到，则重新请求最后一个参数给定的uri，就是新的location匹配

关键点4：如果是格式2，如果最后一个参数是 = 404 ，若给出的file都没有匹配到，则最后返回404的响应码

示例：
```
location / {
	root /opt/nginx/html/;
	try_files $uri $uri/ /index.html;
}
```

请求:http://yhz.test.com/home.html，

$uri为home.html。

会依次查找“/opt/nginx/html/home.html” ， “/opt/nginx/html/home.html/” ；如若都没有查找到，则请求http://yhz.test.com/index.html

.其他注意事项
1.try-files 如果不写上 $uri/，当直接访问一个目录路径时，并不会去匹配目录下的索引页 。
即访问127.0.0.1/images/ 不会去访问 127.0.0.1/images/index.html


# 五、Nginx日志配置


可以应用access_log指令的作用域分别有http，server，location

## Nginx的访问日志

```
设置访问日志
access_log on
access_log path 样式;  
```
自定义日志格式

```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                  '$status $body_bytes_sent "$http_referer" '
                  '"$http_user_agent" "$http_x_forwarded_for"';
                  
access_log /var/logs/nginx-access.log main;
server {
    
}
```

按天设置log日志

if ($time_iso8601 ~ "^(\d{4})-(\d{2})-(\d{2})") {
        set $year $1;
        set $month $2;
        set $day $3;
}


 access_log  /var/logs/xxxx/access/xxxxx_xx_access_$year-$month-$day.log  main;




## Nginx的错误日志

日志格式：

```
error_log file [level];
Default:
error_log logs/error.log error;

```

# 六、SSL证书配置

## nginx安装http_ssl_module模块

使用nginx -V命令可以查看nginx是否安装了对应的模块

没有安装nginx的ssl模块示例如下：

（不包含configure arguments: --prefix=/usr/local/nginx  --with-http_ssl_module）

```
[root@iZbp19n36uysranoj3k2x5Z sbin]# ./nginx -V
nginx version: nginx/1.13.7
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC)
configure arguments: --prefix=/usr/local/nginx --with-http_gzip_static_module
```

如果没有安装ssl模块，可以重新编译一个带有ssl模块的nginx应用程序

步骤一：我们先来到当初下载nginx的包压缩的解压目录.
```
[root@iZbp19n36uysranoj3k2x5Z nginx]# cd nginx-1.13.7
```

步骤二：在nginx目录下执行./config --with-http_ssl_module

```
[root@iZbp19n36uysranoj3k2x5Z nginx]# cd nginx-1.13.7

./configure --with-http_ssl_module //重新添加ssl模块
```

步骤三：执行make命令
```
[root@iZbp19n36uysranoj3k2x5Z nginx]# cd nginx-1.13.7

[root@iZbp19n36uysranoj3k2x5Z nginx]# make //重新编译nginx应用程序
```
**注意：只执行make命令，不执行make install命令，因为make install命令会将nginx的所有配置文件覆盖安装原来的应用。**

步骤四：用新的nginx程序替换原来的nginx程序

在我们执行完做命令后，我们可以查看到在nginx解压目录下，objs文件夹中多了一个nginx的文件，这个就是新版本的程序了。首先我们把之前的nginx先备份一下，然后把新的程序复制过去覆盖之前的即可。
```
cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak

cp objs/nginx /usr/local/nginx/sbin/nginx
```

步骤五：执行 nginx -V检查ssl是否安装成功

```shell
[root@iZbp1hvca0d8vk8fgxk0b5Z sbin]# ./nginx -V
nginx version: nginx/1.17.5
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC)
built with OpenSSL 1.1.1g FIPS  21 Apr 2020
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```

## 复制ssl证书到nginx服务器

在 SSL 证书管理控制台 中选择您需要安装的证书并单击下载。

在弹出的 “证书下载” 窗口中，服务器类型选择 Nginx，单击下载并解压缩 cloud.tencent.com 证书文件包到本地目录。

解压缩后，可获得相关类型的证书文件。其中包含 xxxxxxx.key文件（私钥文件）和xxxxxx.pem文件（证书文件）

将这两个文件上传到nginx的服务器

## 配置nginx的ssl服务
```conf
server {

    #配置HTTPS的默认访问端口为443。
    #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    listen 443 ssl;
   
    
    server_name www.iyuya.com; #需要将yourdomain.com替换成证书绑定的域名。

    root html;

    index index.html index.htm;

    #ssl证书地址
    ssl_certificate cert/5528405__iyuya.com.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。

    ssl_certificate_key cert/5528405__iyuya.com.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。

    ssl_session_timeout 5m;

    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;

    #表示使用的加密套件的类型。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。

    ssl_prefer_server_ciphers on;


      location ~ .* {
	    if ( $host = 'html5.iyuya.com' ) {
                root /data/html5;

            } 
	    
            if ( $host != 'html5.iyuya.com' ) {
                root /data/website/yuya_web/src;

            }
            #root /data/website/yuya_web/src;
            index  index.html index.htm;
            if (!-e $request_filename) {
                rewrite ^/(.*) /index.html last;
                break;
                }
        }
       location ^~ /api/ {
                rewrite  ^/api/(.*)$ /$1 break;
                #proxy_pass   http://127.0.0.1:50011;
                proxy_pass http://teacher.iyuya.com;
        }
    }


```



# 六、其他配置、websocket ssl、gzip等
