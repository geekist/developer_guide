# 一、stub_status简介

该模块用来统计当前的连接状态，如，活动连接数，请求次数、应答次数等。作用是一个监视模块，可以查看目前的连接数等一些信息，因为是非核心模块，nginx默认是没有安装的

## 二、Nginx安装和配置stub_status

## 2.1 查看nginx安装http_stub_status模块

**nginx -V**

使用nginx -V命令可以查看nginx是否安装了对应的模块

如果安装了stub_status模块，在configure arguments中会显示：

**--with-http_stub_status_module**

示例如下：

```
[root@iZbp19n36uysranoj3k2x5Z sbin]# ./nginx -V
nginx version: nginx/1.13.7
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC)
configure arguments: --prefix=/usr/local/nginx --with-http_gzip_static_module  --with-http_stub_status_module
```

## 2.2 编译安装stub_status模块

如果没有安装stub_status模块，可以重新编译一个带有stub_status模块的nginx应用程序

步骤一：我们先来到当初下载nginx的包压缩的解压目录.
```
[root@iZbp19n36uysranoj3k2x5Z nginx]# cd nginx-1.13.7
```

步骤二：在nginx目录下执行./config --with-http_stub_status_module

```
[root@iZbp19n36uysranoj3k2x5Z nginx]# cd nginx-1.13.7

./configure --with-http_stub_status_module //重新添加ssl模块
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

步骤五：执行 nginx -V检查stub_Status是否安装成功

```shell
[root@iZbp1hvca0d8vk8fgxk0b5Z sbin]# ./nginx -V
nginx version: nginx/1.17.5
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC)
built with OpenSSL 1.1.1g FIPS  21 Apr 2020
TLS SNI support enabled
configure arguments: --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```

## 2.3 配置stub_status
在server或location端添加配置信息：

```shell
location /nginx_status {
    # Turn on nginx stats
    stub_status on;
    # I do not need logs for stats
    access_log   off;
    # Security: Only allow access from 192.168.1.100 IP #
    #allow 192.168.1.100;
    # Send rest of the world to /dev/null #
    #deny all;
}
```


```shell
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

   location /status {
			stub_status on;
			access_log on;
			allow 127.0.0.1;
			deny all;
		}

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }


    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
```

## 2.4 访问监控信息

采用http方式访问：
http://127.0.0.1/nginx_status

返回结果如下：

1）Active connections-活跃连接数

The current number of active client connections including Waiting connections.

（2）accepts-已接受的客户端连接总数

The total number of accepted client connections.

（3）handled-已处理的连接总数

The total number of handled connections. Generally, the parameter value is the same as acceptsunless some resource limits have been reached (for example, the worker_connections limit).

（4）requests-客户端连接总数

The total number of client requests.

（5）Reading-读取请求头的当前连接数

The current number of connections where nginx is reading the request header.

（6）Writing-将响应写回客户端的当前连接数

The current number of connections where nginx is writing the response back to the client.

（7）Waiting-等待请求的当前空闲客户端连接数

The current number of idle client connections waiting for a request.