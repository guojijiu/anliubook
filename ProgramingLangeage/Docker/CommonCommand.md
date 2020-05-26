# docker常用命令

1. 安装php容器

```
docker run -p 9000:9000 --privileged=true --name php7.4 -v /docker/www:/www -v /docker/php/conf:/usr/local/etc/php -v /docker/php/log:/phplogs -d php:7.4-fpm
```

2. 安装nginx容器

```
docker run -p 80:80 --privileged=true --name nginx -v /docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /docker/nginx/conf/conf.d/:/etc/nginx/conf.d/ -v /docker/www:/web -v /docker/nginx/log:/var/log/nginx -v /docker/nginx/log:/wwwlogs -d nginx
```

3. 安装mysql容器

```
docker run --name mysql5.7 -p 3306:3306 --privileged=true -v /docker/mysql/conf:/etc/mysql/conf.d -v /docker/mysql/log:/logs -v /docker/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```

登录mysql获取权限：

```
docker exec -it mysql5.7 bash
mysql -uroot -p
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
4. 安装redis容器
```
 docker run -p 6379:6379 --name redis5 -v /docker/redis/conf:/etc/redis/redis.conf -v /docker/redis/data:/data -d redis:5 redis-server /etc/redis/redis.conf --appendonly yes
```
5. 安装mongodb容器
```
docker run --name mongodb4 -p 27017:27017 -v /docker/mongo/data:/data/db -d mongo:4 --auth
```
创建一个登录用户
```
docker exec -it mongodb4 mongo admin
db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'userAdminAnyDatabase', db: 'admin'}]});
db.auth('admin', '123456')
```
6. 安装python容器
```
docker run -itd -v /docker/python/app:/usr/src/python/app -w /usr/src/python/app --name=python3.8 -d python:3.8
```
7. 查看容器ip
```
docker inspect php |grep '"IPAddress"'
```
8. 查看容器日志
```
docker inspect php |grep '"IPAddress"'
```
9. 获取所有的容器IP
```
docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq)
```