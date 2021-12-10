
## yuya_nginx_conf

```java

#----------------------------------------------------------------------------
#配置服务器用户（组）的指令是user
#user user [group]
#第一个user是指令
#第二个user是指定可以运行Nginx服务器的用户
#group是可选项，指定可以运行服务器的用户组
#只有设置的用户或用户组才有权限启动Nginx服务进程。

#如果未配置或配置设置为
#user nobody nobody;
#表示所有用户都可以启动Nginx进程
#---------------------------------end of explaination -----------------------
#user  nobody;



#------------------------------------------------------------------------------------
#worker process 是Nginx服务器实现并发处理服务的关键所在。
#从理论上讲值越大，可以支持的并发处理也越多。但实际还要受软件本身、操作系统本身的资源和能力、硬件设备等限制。

#配置方式如下：

#worker_process number | auto
#number表示指定nginx进程最多可以产生的worker process 数量
#auto表示nginx进程自动检测

#在默认配置中woker_process 配置为1
#---------------------------------end of explaination -----------------------

worker_processes  1;

#----------------------------------------------------------------------------------------
#错误日志配置
在全局块，http块和server块都可以对nginx服务器的日志进行相关的配置。此处只介绍全局块下的日志存放配置。
从默认配置中可以看到，使用指令error_log即可以配置日志的输出 并且可以指定日志级别

error_log file | stderr [debug | info | notice | warn | error | crit | alert | emerg]
#--------------------------------end of explaination ---------------------------------

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#--------------------------------------------------------------------------------------
#进程PID设置
#nginx进程作为系统的守护进程运行，我们需要在某个文件中板寸当前运行程序的主进程号。
#默认启动nginx服务器，进程pid默认放在nginx安装目录下的logs目录下的，名字为nginx.pid。


#nginx.pid文件可以通过在主配置文件中通过pid指令配置到其他位置并修改名字
#如以下配置：

#该配置还是指向默认的位置，但是可以自定义修改。
#并且该指令只能在全局块中配置。
#--------------------------------end of explaination ---------------------------------

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    client_max_body_size 10m;
    #gzip  on;
     

  server {
    listen 443 ssl;
    #配置HTTPS的默认访问端口为443。
    #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
    #如果您使用Nginx 1.15.0及以上版本，请使用listen 443 ssl代替listen 443和ssl on。
    server_name www.iyuya.com; #需要将yourdomain.com替换成证书绑定的域名。
    root html;
    index index.html index.htm;
    ssl_certificate cert/5528405__iyuya.com.pem;  #需要将cert-file-name.pem替换成已上传的证书文件的名称。
    ssl_certificate_key cert/5528405__iyuya.com.key; #需要将cert-file-name.key替换成已上传的证书密钥文件的名称。
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    #表示使用的加密套件的类型。
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #表示使用的TLS协议的类型。
    ssl_prefer_server_ciphers on;
      location ~ .* {
            root /data/website/yuya_web/src;
            index  index.html index.htm;
            if (!-e $request_filename) {
                rewrite ^/(.*) /index.html last;
                break;
                }
        }

    }

     #===============官网模块 ===========================
    server {

         listen 80;
         server_name www.iyuya.com;
         access_log /data/wwwlogs/access_nginx.log combined;
         gzip on;
        gzip_buffers 32 4K;
        gzip_comp_level 6;
        gzip_min_length 100;
        gzip_types application/javascript text/css text/xml;
        gzip_disable "MSIE [1-6]\."; #配置禁用gzip条件，支持正则。此处表示ie6及以下不启用gzip（因为ie低版本不支持）
        gzip_vary on;
	
	rewrite ^(.*)$ https://$host$1; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
        location ~ .* {
            root /data/website/yuya_web/src;
            index  index.html index.htm;
            if (!-e $request_filename) {
                rewrite ^/(.*) /index.html last;
                break;
                }
        }

    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
      expires 30d;
      access_log off;
    }

    location ~ .*\.(js|css)?$ {
      expires 7d;
      access_log off;
    }

     location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
               deny all;
      }
    }
  


    #===============管理后台模块 =========================== 
    server {
   	 listen 80;
         server_name biz.iyuya.com;
    	 access_log /data/wwwlogs/access_nginx.log combined;
    	 root /var/yuya/yuya_admin/yuya-admin/src/main/resources/static;
	 #root /var/oldyuya/yuya-biz/yuya-admin/src/main/resources/static;    	 
#error_page 404 /404.html;
    	 #error_page 502 /502.html;
    	 location /nginx_status {
      	 stub_status on;
      	 access_log off;
      	 allow 127.0.0.1;
      	 deny all;
    }
    location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
      expires 30d;
      access_log off;
    }
    location ~ .*\.(js|css)?$ {
      expires 7d;
      access_log off;
    }

    location ~ {
      proxy_pass http://127.0.0.1:8077;
      include proxy.conf;
    }
    	location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
      		deny all;
  	  }
    }
    #===============老师模块 =========================== 
    server {
    	listen 80;
    	server_name teacher.iyuya.com;
    	access_log /data/wwwlogs/access_nginx.log combined;
    	root /var/yuya/yuya_teachers/target/yuya-release/yuya/webapp;
        #root /var/oldyuya/teacher/target/yuya-release/yuya/webapp;

    	#error_page 404 /404.html;
    	#error_page 502 /502.html;
    	location /nginx_status {
      		stub_status on;
      		access_log off;
      		allow 127.0.0.1;
      		deny all;
        }
    	location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
      		expires 30d;
      		access_log off;
    	}
    	location ~ .*\.(js|css)?$ {
      		expires 7d;
      		access_log off;
    	}
    	location ~ {
      		proxy_pass http://127.0.0.1:8088;
      		include proxy.conf;
    	}
    		location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
      		deny all;
    	}
   }

   #===============家长模块 =========================== 
   server {
       listen 80;
       server_name parent.iyuya.com;
       access_log /data/wwwlogs/access_nginx.log combined;
       root /var/yuya/yuya_parent/target/yuya-release/yuya/webapp;
       #root /var/oldyuya/parent/target/yuya-release/yuya/webapp;
       #error_page 404 /404.html;
       #error_page 502 /502.html;
       location /nginx_status {
      		stub_status on;
      		access_log off;
      		allow 127.0.0.1;
      		deny all;
       }
       location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
      		expires 30d;
      		access_log off;
       }
       location ~ .*\.(js|css)?$ {
      		expires 7d;
      		access_log off;
       }
       location ~ {
      		proxy_pass http://127.0.0.1:8089;
      		include proxy.conf;
       }
       location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
     	 	deny all;
       }
   }

   #===============消息模块 =========================== 
   server {
       listen 80;
       server_name message.iyuya.com;
       access_log /data/wwwlogs/access_nginx.log combined;
       root /var/yuya/yuya_message/target/yuya-release/yuya/webapp;
       #root /var/oldyuya/message/target/yuya-release/yuya/webapp;

       #error_page 404 /404.html;
       #error_page 502 /502.html;
       location /nginx_status {
       		stub_status on;
      		access_log off;
      		allow 127.0.0.1;
      		deny all;
       }
       location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
      		expires 30d;
      		access_log off;
       }
       location ~ .*\.(js|css)?$ {
     	 	expires 7d;
      		access_log off;
       }
       location ~ {
      		proxy_pass http://127.0.0.1:8099;
      		include proxy.conf;
       }
       location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
      		deny all;
       }
   }


    #===============教案中心/家长课堂H5 =========================== 
    server {
        listen       80;
        server_name  html5.qilaimi.com;


      	location ~ .* {
            root /data/html5;
            index  index.html index.htm;
            if (!-e $request_filename) {
                rewrite ^/(.*) /index.html last;
                break;
                }
        }

        location ^~ /api/parent/ {
                rewrite  ^/api/(.*)$ /$1 break;
                proxy_pass   http://127.0.0.1:8089;
        }

        location ^~ /api/teacher/ {
               rewrite  ^/api/(.*)$ /$1 break;
               proxy_pass   http://127.0.0.1:8088;
        }

        location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
                expires 30d;
                access_log off;
        }
        location ~ .*\.(js|css)?$ {
                expires 7d;
        				access_log off;
        }
        location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
                deny all;
        }
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}


```