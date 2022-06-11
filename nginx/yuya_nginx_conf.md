
#yuya_nginx.conf_20220611

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

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

    client_max_body_size 50m;
    #gzip  on;
     proxy_intercept_errors on;
     fastcgi_intercept_errors on;

    
# 开启gzip
gzip  on;
# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k;
# gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间。一般设置1和2
gzip_comp_level 2;
# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;
# 禁用IE 6 gzip
gzip_disable "MSIE [1-6]\.";
# 设置缓存路径并且使用一块最大100M的共享内存，用于硬盘上的文件索引，包括文件名和请求次数，每个文件在1天内若不活跃（无请求）则从硬盘上淘汰，硬盘缓存最大10G，满了则根据LRU算法自动清除缓存。
proxy_cache_path /var/cache/nginx/cache levels=1:2 keys_zone=imgcache:100m inactive=1d max_size=10g;

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

	location /api {
            rewrite ^/api/(.*)$ /$1 break;
            proxy_pass http://8.136.150.121:50011;

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
         root   /home/web;

    	 error_page 404 500 502 503 504  /502.html;
    	 
         location = /502.html {
            root   /data/maintain; # 页面所在的文件夹位置，注意是文件夹位置，而不是页面路径
         } 

	location /api {
            rewrite ^/api/(.*)$ /$1 break;
            proxy_pass http://8.136.150.121:60011;

          }
        
         location / {
            try_files $uri $uri/ /index.html;
          }


    }


server {
    listen 60011;
    server_name teacher.qilaimi.com;
    access_log /data/wwwlogs/access_nginx.log combined;
    root /var/yuya/teachers/target/yuya-release/yuya/webapp;
    #error_page 404 /404.html;
    #error_page 502 /502.html;

                location ~* ^(/v2|/webjars|/swagger-resources|/swagger-ui.html|/doc.html)   {
         proxy_pass http://127.0.0.1:60012;
       #index index.html index.htm;
       client_max_body_size 300m;
    }


       location /websocket {
     proxy_pass http://127.0.0.1:60012;
     #注意：使用代理地址时末尾记得加上斜杠"/"。
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_read_timeout 600s;

   }



    location /  {
                proxy_pass http://127.0.0.1:60012;
      #index index.html index.htm;
      client_max_body_size 300m;
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
      		proxy_pass http://127.0.0.1:50011;
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
      		proxy_pass http://127.0.0.1:50011;
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
      		proxy_pass http://127.0.0.1:50011;
      		include proxy.conf;
       }
       location ~ ^/(\.user.ini|\.ht|\.git|\.svn|\.project|LICENSE|README.md) {
      		deny all;

       }
   }


    #===============教案中心/家长课堂H5 =========================== 
    server {
        listen       80;
        server_name  html5.iyuya.com;
	
	 location  ^~ /api/ {
                   proxy_pass   http://8.136.150.121:50011/;
     }

	
      	location ~ .* {
            root /data/html5;
            index  index.html index.htm;
            if (!-e $request_filename) {
                rewrite ^/(.*) /index.html last;
                break;
             }
	    
           # return 702;
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
