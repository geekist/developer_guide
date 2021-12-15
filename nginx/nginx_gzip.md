

## nginx如何设置gzip压缩



```
server{
    gzip on;
    gzip_buffers 32 4K;
    gzip_comp_level 6;
    gzip_min_length 100;
    gzip_http_version 1.1;
    gzip_types application/javascript text/css text/xml;
    gzip_disable "MSIE [1-6]\."; #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
    gzip_vary on;
}
```

* ***gzip on | off;***


打开或关闭gzip

Syntax: gzip on | off;

Default:    gzip off;

Context:    http, server, location, if in location



* ***gzip_buffers***


设置用于处理请求压缩的缓冲区数量和大小。比如32 4K表示按照内存页（one memory page）大小以4K为单位（即一个系统中内存页为4K），申请32倍的内存空间。建议此项不设置，使用默认值。

Syntax: gzip_buffers number size;

Default:    gzip_buffers 32 4k|16 8k;

Context:    http, server, location

* ***gzip_comp_level***


设置gzip压缩级别，级别越底压缩速度越快文件压缩比越小，反之速度越慢文件压缩比越大;

一方面，不是压缩级别越高越好，其实gzip_comp_level 1的压缩能力已经够用了，后面级别越高，压缩的比例其实增长不大，反而很吃处理性能。
另一方面，压缩一定要和静态资源缓存相结合，缓存压缩后的版本，否则每次都压缩高负载下服务器肯定吃不住。

Syntax: gzip_comp_level level;

Default:    gzip_comp_level 1;

Context:    http, server, location


* ***gzip_disable***


通过表达式，表明哪些UA头不使用gzip压缩

Syntax: gzip_disable regex ...;

Default:    —

Context:    http, server, location

* ***gzip_min_length***


当返回内容大于此值时才会使用gzip进行压缩,以K为单位,当值为0时，所有页面都进行压缩。

Syntax: gzip_min_length length;

Default:    gzip_min_length 20;

Context:    http, server, location

* ***gzip_http_version***


用于识别http协议的版本，早期的浏览器不支持gzip压缩，用户会看到乱码，所以为了支持前期版本加了此选项。默认在http/1.0的协议下不开启gzip压缩。

Syntax: gzip_http_version 1.0 | 1.1;

Default:    gzip_http_version 1.1;

Context:    http, server, location

* ***gzip_proxied***


Nginx做为反向代理的时候启用：

off – 关闭所有的代理结果数据压缩

expired – 如果header中包含”Expires”头信息，启用压缩

no-cache – 如果header中包含”Cache-Control:no-cache”头信息，启用压缩

no-store – 如果header中包含”Cache-Control:no-store”头信息，启用压缩

private – 如果header中包含”Cache-Control:private”头信息，启用压缩

no_last_modified – 启用压缩，如果header中包含”Last_Modified”头信息，启用压缩

no_etag – 启用压缩，如果header中包含“ETag”头信息，启用压缩

auth – 启用压缩，如果header中包含“Authorization”头信息，启用压缩

any – 无条件压缩所有结果数据

Syntax: gzip_proxied off | expired | no-cache | no-store | private | no_last_modified | no_etag | auth | any ...;

Default:    gzip_proxied off;

Context:    http, server, location

* ***gzip_types***


设置需要压缩的MIME类型,如果不在设置类型范围内的请求不进行压缩

Syntax: gzip_types mime-type ...;

Default:    gzip_types text/html;

Context:    http, server, location

|--字体类型扩展名--|--Content-type--|
.eot	application/vnd.ms-fontobject
.ttf	font/ttf
.otf	font/opentype
.woff	font/x-woff
.svg	image/svg+xml









