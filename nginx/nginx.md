
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
    listen 127.0.0.1:8080;
    # 其他配置
}
```

如果不填写端口，则采用标准端口。

如果不填写ip地址，则监听所有地址。

如果缺少整条listen指令，则标准端口是80/tcp，默认端口是8000/tcp，由超级用户的权限决定。

如果有多个server配置了相同的ip地址和端口，Nginx会匹配server_name指令与请求头部的host字段。

server_name:
server_name指令的参数可以是精确的文本、通配符或正则表达式。通配符可以在字符串的头部、尾部或两端包含*，*可以匹配任意字符。Nginx采用Perl格式的正则表达式，以~开头。以下是一个精确匹配的例子：

```
server {
    listen      80;
    server_name example.org www.example.org;
    ...
}
```

如果有多个server_name匹配host字段，Nginx根据以下规则选择第一个相匹配的server处理请求：

精确匹配
以 * 开始的最长通配符，如 *.example.org
以 * 结尾的最长通配符，如 mail. *
第一个匹配的正则表达式（根据在配置文件中出现的先后顺序）

如果找不到任何与host字段相匹配的server_name，Nginx会根据请求端口将其发送给默认的server。默认server就是

配置文件中第一个出现的server，也可以通过default_server指定某个server为默认server

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

大致的意思是，当你访问 www.yayujs.com 的 80 端口的时候，返回 /home/www/ts/index.html 文件。

#### localtion 语法

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

（1） “=” ，精确匹配

```
内容要同表达式完全一致才匹配成功
location = /abc/ {
  .....
 }
        
# 只匹配http://abc.com/abc
#http://abc.com/abc [匹配成功]
#http://abc.com/abc/index [匹配失败]
```

（2） “~”，执行正则匹配，区分大小写。

```
location ~ /Abc/ {
  .....
}

#http://abc.com/Abc/ [匹配成功]
#http://abc.com/abc/ [匹配失败]
```

（3）“~*”，执行正则匹配，忽略大小写

```
location ~* /Abc/ {
  .....
}

# 则会忽略 uri 部分的大小写
#http://abc.com/Abc/ [匹配成功]
#http://abc.com/abc/ [匹配成功]
```

（4）“^~”，表示普通字符串匹配上以后不再进行正则匹配。

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
```解释：

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

```
访问根目录/， 比如http://localhost/ 将匹配规则A

访问 http://localhost/login 将匹配规则B，http://localhost/register 则匹配规则H

访问 http://localhost/static/a.html 将匹配规则C

访问 http://localhost/b.jpg 将匹配规则D和规则E，但是规则D顺序优先，规则E不起作用， 而 http://localhost/static/c.png 则优先匹配到 规则C

访问 http://localhost/a.PNG 则匹配规则E， 而不会匹配规则D，因为规则E不区分大小写。

访问 http://localhost/a.xhtml 不会匹配规则F和规则G，http://localhost/a.XHTML不会匹配规则G，因为不区分大小写。规则F，规则G属于排除法，符合匹配规则但是不会匹配到。

访问 http://localhost/qll/id/1111 则最终匹配到规则H，因为以上规则都不匹配。
```


#### root index 、alias和return

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

#### return

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

* 第一种情况：proxy_pass结尾有IP+port+/

```
location /online/wxapi/ {
        proxy_pass http://localhost:8080/;
        proxy_set_header X-Real-IP $remote_addr;
}
```
proxy_pass后面有斜杠，则拼接不包含前缀：

代理后的实际地址：http://localhost:8080/test/loginSwitch

* 第二种情况: proxy_pass最后没有/
```
location /online/wxapi/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
}
```
IP+Port：拼接包含前缀

代理后的实际地址：http://localhost:8080/online/wxapi/test/loginSwitch

* 第三种情况:proxy_pass最后有/web

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



# 五、其他配置、websocket ssl、gzip等

## [Linux环境下Nginx安装和配置](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_install_linux.md)

## [Windows环境下Nginx安装和配置](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_install.md)

## [nginx配置文件详解](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_config.md)

## [nginx使用gzip压缩](./nginx_gzip.md)

## [YuYa_Nginx配置](https://github.com/geekist/developer_guide/blob/main/nginx/yuya_nginx_conf.md)

## [nginx常用命令和疑难问题解决](https://github.com/geekist/developer_guide/blob/main/nginx/nginx_command.md)

