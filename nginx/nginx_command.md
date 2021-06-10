
## nginx常用命令

### 运行nginx

如果nginx没有运行，则运行nginx

start nginx 

如果nginx已经运行，则重新启动nginx

nginx -s reload

### 终止nginx

快速停止nginx

nginx -s stop

完整有序的关闭

nginx -s quit

### 检查配置文件

nginx -t -c /nginx-1.15.2/conf/nginx.conf