#docker常用命令
1. 安装php容器
```
docker run -p 9000:9000 --name  php -v /docker/www/laravel:/www -v /docker/php/conf:/usr/local/etc/php -v /docker/php/log:/phplogs   -d php:7.4-fpm
```
2. 安装nginx容器
```
docker run -p 80:80 --name nginx -v /docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /docker/nginx/conf/conf.d/:/etc/nginx/conf.d/ -v /docker/www:/www -v /docker/nginx/log:/var/log/nginx -v /docker/nginx/log:/wwwlogs -d nginx
```
3. 查看容器ip
```
docker inspect php |grep '"IPAddress"'
```
4. 查看容器日志
```
docker inspect php |grep '"IPAddress"'
```
