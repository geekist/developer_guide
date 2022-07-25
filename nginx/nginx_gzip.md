# 一、gzip简介

## 1.1 gzip介绍

Nginx是一款性能强大的服务器，提供了很多对优化 Web 性能有帮助的功能配置。对静态资源提供 gzip 压缩就是其中之一。启用 gzip 压缩可大幅缩减所传输的资源文件响应的大小（最多可缩减 90%），服务器将向客户端发送较小的资源，从而显著缩短下载相应资源所需的时间、减少客户端的流量消耗并加快网页的首次呈现速度。所以在服务器端启用 gzip 压缩，也是一个非常有效的前端性能优化手段。

gzip是GNUzip的缩写，最早用于UNIX系统的文件压缩。HTTP协议上的gzip编码是一种用来改进web应用程序性能的技术，web服务器和客户端（浏览器）必须共同支持gzip。目前主流的浏览器，Chrome,firefox,IE等都支持该协议。常见的服务器如Apache，Nginx，IIS同样支持gzip。

gzip压缩比率在3到10倍左右，可以大大节省服务器的网络带宽。而在实际应用中，并不是对所有文件进行压缩，通常只是压缩静态文件。

在服务器端（nginx）启用 gzip 压缩，网站对于网络带宽的需求也降低了，或者说是在现有的带宽情况下，能够更加充分的利用带宽资源，从长期效益来看，也可以间接的降低公司在带宽上的运行成本。

## 1.2 gzip工作原理

1)浏览器请求url，并在request header中设置属性accept-encoding:gzip。表明浏览器支持gzip。

2)服务器收到浏览器发送的请求之后，判断浏览器是否支持gzip，如果支持gzip，则向浏览器传送压缩过的内容，不支持则向浏览器发送未经压缩的内容。一般情况下，浏览器和服务器都支持gzip，response headers返回包含content-encoding:gzip。

3)浏览器接收到服务器的响应之后判断内容是否被压缩，如果被压缩则解压缩显示页面内容。

# 二、gzip压缩类型、压缩比、最小压缩值


## 2.1 gzip压缩类型

gzip 主要用于对文本类型的资源进行压缩，例如常用见的文本资源：

HTML 文件：text/html（nginx 服务器默认就会压缩）、application/xhtml+xml
CSS 文件：text/css
JS 文件：application/x-javascript、application/javascript、text/javascript
JSON 文件（或者API请求结果）：application/json、application/geo+json、application/ld+json application/manifest+json、application/x-web-app-manifest+json
XML 文件：application/xml、application/atom+xml、application/rdf+xml、application/rss+xml
SVG 文件：image/svg+xml;

除了常用的文本文件，gzip 也支持压缩以下 MIME 类型的文件：

application/vnd.ms-fontobject
application/wasm
font/eot
font/otf
font/ttf
image/bmp
text/cache-manifest
text/calendar
text/markdown
text/plain
text/vcard
text/vnd.rim.location.xloc
text/vtt
text/x-component
text/x-cross-domain-policy

GZip 对基于文本的内容的资源压缩效果最好，在压缩较大文件时往往可实现高达 70-90% 的压缩率，而如果对已经通过替代算法压缩过的资源（例如，大多数图片格式）运行 gzip，则效果甚微，甚至毫无效果。

## 2.2 启用合适的压缩比

Nginx 服务器中 gzip 压缩比的配置项是：gzip_comp_level，可选的值为 1-9，数值越高压缩比也就越高。但实际的应用实践中需要按照自己的实际情况来配置压缩比。因为启用 gzip 压缩，主要消耗的是服务器的 CPU 资源。压缩比越高，服务器对 CPU 资源的消耗也就越高。如果服务器的 CUP 配置不高，采用过高的压缩比反而不好，可能会导致服务器 CPU 的占用率太高，从而导致 NGINX 变慢或者卡顿。

如果没有特别高的要求，建议的 gziip 压缩比值为2，即可保证压缩的效果（对大多 ASCII 编码的文件可以达到75%的压缩比），同时对 CPU 资源的损耗也比较低。并且通过在 DevOps 平台的实践中实测的结果，压缩比设置为 6-9，压缩的效果与 5 相比效果并不明显，只会增加对 CPU 资源的消耗所以 DevOps 平台 nginx 服务器 gzip 配置的压缩等级也为 2。

## 2.3 启用gzip压缩文件的最小体积

最后,，nginx 服务器对 gzip 的配置项中，有一项是：gzip_min_length， 用来配置启用 gzip 压缩文件的最小体积。因为 gzip 压缩对原本体积就很小的资源文件压缩的效果也并不好，甚至可能出现使用 gzip 压缩体积很小的文件，压缩后的体积反而比压缩之前更大。所以通常会设置 gzip_min_length 1k，只有文件的体积超过了 1k，服务器才启用 gzip 压缩此文件。

# 三、Nginx安装和配置gzip

## 3.1 确定 Nginx 服务器是否支持 GZip

**nginx -V**

如果你是使用 CentOS （本文以 CentOS 为例）或者使用其它 linux 服务器，通过包管理工具（yum、apt-get、dnf等）安装的 nginx，默认就开启了对 gzip 模块的支持。

可以使用：nginx -V 命令，查看 nginx 服务器是否开启了对 gzip 的支持模块：

```shell
[root@geekist local]# nginx -V
nginx version: nginx/1.20.1
built by gcc 10.2.1 20200825 (Alibaba 10.2.1-3 2.32) (GCC)
built with OpenSSL 1.1.1g FIPS  21 Apr 2020 (running with OpenSSL 1.1.1k  FIPS 25 Mar 2021)
TLS SNI support enabled
configure arguments: --prefix=/usr/share/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --http-client-body-temp-path=/var/lib/nginx/tmp/client_body --http-proxy-temp-path=/var/lib/nginx/tmp/proxy --http-fastcgi-temp-path=/var/lib/nginx/tmp/fastcgi --http-uwsgi-temp-path=/var/lib/nginx/tmp/uwsgi --http-scgi-temp-path=/var/lib/nginx/tmp/scgi --pid-path=/run/nginx.pid --lock-path=/run/lock/subsys/nginx --user=nginx --group=nginx --with-file-aio --with-ipv6 --with-http_ssl_module --with-http_v2_module --with-http_realip_module --with-stream_ssl_preread_module --with-http_addition_module --with-http_xslt_module=dynamic --with-http_image_filter_module=dynamic --with-http_sub_module --with-http_dav_module --with-http_flv_module --with-http_mp4_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_random_index_module --with-http_secure_link_module --with-http_degradation_module --with-http_slice_module --with-http_stub_status_module --with-http_perl_module=dynamic --with-http_auth_request_module --with-mail=dynamic --with-mail_ssl_module --with-pcre --with-pcre-jit --with-stream=dynamic --with-stream_ssl_module --with-debug --with-cc-opt='-O2 -g -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fexceptions -fstack-protector-strong -grecord-gcc-switches -specs=/usr/lib/rpm/redhat/redhat-hardened-cc1 -specs=/usr/lib/rpm/redhat/redhat-annobin-cc1 -m64 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection -floop-unroll-and-jam -ftree-loop-distribution --param early-inlining-insns=160 --param inline-heuristics-hint-percent=800 --param inline-min-speedup=50 --param inline-unit-growth=256 --param max-average-unrolled-insns=500 --param max-completely-peel-times=32 --param max-completely-peeled-insns=800 --param max-inline-insns-auto=128 --param max-inline-insns-small=128 --param max-unroll-times=16 --param max-unrolled-insns=16' --with-compat --with-ld-opt='-Wl,-z,relro -Wl,-z,now -specs=/usr/lib/rpm/redhat/redhat-hardened-ld -Wl,-E'
```

nginx 中的 gzip 处理模块是：ngx_http_gzip_module。如果显示如上图所示的：–with-http_gzip_static_module，就说明你的nginx服务器已经支持 gzip 了，可以开始配置 gzip 压缩了。

## 3.2 编译安装Nginx的gzip模块

**./configure  --with-http_gzip_static_module**

在nginx的目录下，执行configure，并将gzip模块添加到参数中。

注意：

如果是全新安装nginx，则可以使用make && make install。

如果Nginx已经安装，本次只是添加gzip模块，则只需要执行make，然后将编译出来的nginx程序（objs目录下）拷贝到sbin目录下替换原有的nginx，然后执行nginx -s reload重新启动


## 3.3 配置 GZip 压缩

在确定支持 gzip 后，就可以进行 gzip 压缩相关的配置了。使用 vim 编辑器打开 nginx.conf 配置文件：

sudo vim /usr/local/nginx/nginx.conf
打开文件后，按 i 键进入编辑模式，在 http 块中输入以下配置信息：


```shell
http {
  #=====================gzip配置=========================================
  #gzip 可以在 http, server, location 中和配置，这里配置到 http 下是全局配置，
  #只要是使用当前 nginx 服务器的站点都会开启 gzip   
  
  #开启 gzip，Default: off
  gzip on;

  #压缩级别： 1-9；2 是推荐的压缩级别，Default: 1
  gzip_comp_level 2;

  #gzip 压缩文件体积的最小值。Default: 20
  gzip_min_length 1k;

  #设置用于压缩响应的 number 和 size 的缓冲区。默认情况下，缓冲区大小等于一个内存页。根据平台的不同，它也可以是4K或8K。
  gzip_buffers 4 16k;

  ##是否开启对代理资源的压缩。很多时候，nginx 会作为反向代理服务器可能不存储资源，只有开启了 gzip_proxied 才会对代理的资源进行压缩。Default: off
  gzip_proxied any;

  #每当客户端的 Accept-Encoding-capabilities 头发生变化时，告诉代理缓存 gzip 和常规版本的资源。
  #避免了不支持 gzip 的客户端（这在今天极为罕见）在代理给它们 gzip 版本时显示乱码的问题。
  #如果指令gzip， gzip_static 或 gunzip 处于活动状态， 则启用或禁用插入“ Vary：Accept-Encoding”响应标头字段。Default: off
  gzip_vary on;

  #压缩文件的 MIME 类型。text/html 默认就会开启 gzip 压缩，所以不用特别显示配置。Default: text/html
  gzip_types
    application/javascript
    application/x-javascript
    text/javascript
    text/css
    text/xml
    application/xhtml+xml
    application/xml
    application/atom+xml
    application/rdf+xml
    application/rss+xml
    application/geo+json
    application/json
    application/ld+json
    application/manifest+json
    application/x-web-app-manifest+json
    image/svg+xml
    text/x-cross-domain-policy;
  #服务器开启对静态文件（ CSS, JS, HTML, SVG, ICS, and JSON）的压缩。
  #需要在 gzip_types 设置 MIME 类型，，仅仅设置 gzip_static 为 on 是不会自动压缩静态文件的。
  gzip_static on;  

  #IE6 以下的浏览器禁用 gzip 压缩。
  gzip_disable "MSIE [1-6]\.";  
 
  #设置缓存路径并且使用一块最大100M的共享内存，用于硬盘上的文件索引，包括文件名和请求次数
  #每个文件在1天内若不活跃（无请求）则从硬盘上淘汰，硬盘缓存最大10G，满了则根据LRU算法自动清除缓存。
  proxy_cache_path /var/cache/nginx/cache levels=1:2 keys_zone=imgcache:100m inactive=1d max_size=10g;

}
```
