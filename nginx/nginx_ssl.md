# 一、SSL简介


## 二、Nginx安装和配置ssl

## 2.1 查看nginx安装http_ssl_module模块

**nginx -V**

使用nginx -V命令可以查看nginx是否安装了对应的模块

没有安装nginx的ssl模块示例如下：

```
[root@iZbp19n36uysranoj3k2x5Z sbin]# ./nginx -V
nginx version: nginx/1.13.7
built by gcc 8.3.1 20191121 (Red Hat 8.3.1-5) (GCC)
configure arguments: --prefix=/usr/local/nginx --with-http_gzip_static_module
```

## 2.2 编译安装ssl模块

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

## 2.3 复制ssl证书到nginx服务器

在 SSL 证书管理控制台 中选择您需要安装的证书并单击下载。

在弹出的 “证书下载” 窗口中，服务器类型选择 Nginx，单击下载并解压缩 cloud.tencent.com 证书文件包到本地目录。

解压缩后，可获得相关类型的证书文件。其中包含 xxxxxxx.key文件（私钥文件）和xxxxxx.pem文件（证书文件）

将这两个文件上传到nginx的服务器

## 2.4 配置nginx的ssl服务
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