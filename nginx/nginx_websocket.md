# 一、websocket介绍

WebSocket协议相比较于HTTP协议成功握手后可以多次进行通讯，直到连接被关闭。但是WebSocket中的握手和HTTP中的握手兼容，它使用HTTP中的Upgrade协议头将连接从HTTP升级到WebSocket。这使得WebSocket程序可以更容易的使用现已存在的基础设施。

WebSocket工作在HTTP的80和443端口并使用前缀ws://或者wss://进行协议标注，在建立连接时使用HTTP/1.1的101状态码进行协议切换，当前标准不支持两个客户端之间不借助HTTP直接建立Websocket连接。

# 二、在nginx中配置websocket

配置websocket，需要在location字段中增加如下配置：

```shell
  proxy_http_version 1.1;

  proxy_set_header Upgrade $http_upgrade;

  proxy_set_header Connection "upgrade";

  proxy_read_timeout 600s;
```

```shell
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

```