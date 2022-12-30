# docker常用命令

1. 安装php容器

```
docker run --name php-fpm -v /docker/www:/var/www/html -d php:7.4-fpm
```

2. 安装nginx容器

```
docker run --name nginx -p 80:80 -d -v /docker/www:/usr/share/nginx/html -v /docker/nginx/conf:/etc/nginx/conf -v /docker/nginx/conf/conf.d:/etc/nginx/conf.d -v /docker/nginx/log:/var/log/nginx --link php-fpm:php nginx
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

```
docker-compose build 构建项目中的镜像，–force-rm：删除构建过程中的临时容器；–no-cache：不使用缓存构建；–pull：获取最新版本的镜像
docker-compose up -d 构建镜像、创建服务和启动项目，-d表示后台运行
docker-compose run ubuntu ls -d 指定服务上运行一个命令，-d表示后台运行
docker-compose logs 查看服务容器输出日志
docker-compose ps 列出项目中所有的容器
docker-compose pause [service_name] 暂停一个服务容器
docker-compose unpause [service_name] 恢复已暂停的一个服务容器
docker-compose restart 重启项目中的所有服务容器（也可以指定具体的服务）
docker-compose stop 停止运行项目中的所有服务容器（也可以指定具体的服务）
docker-compose start 启动已经停止项目中的所有服务容器（也可以指定具体的服务）
docker-compose rm 删除项目中的所有服务容器（也可以指定具体的服务），-f：强制删除（包含运行的）
docker-compose kill 强制停止项目中的所有服务容器（也可以指定具体的服务）
```

清空日志
```
1. 查看日志所在位置：docker inspect --format='{{.LogPath}}' <container_name>
2. 清空日志：cat /dev/null > $log
```

非root用户使用docker命令
```
方案一：（验证，生效）
chmod 666 /var/run/docker.sock
方案二：（验证，未生效）
参考官方说明，使用 root 权限创建一个 docker 组，并将普通用户加入到该组中，然后刷新一下 docker 组使其修改生效即可：
sudo groupadd docker			# 有则不用创建
sudo usermod -aG docker USER	# USER 为加入 docker 组的用户
newgrp docker					# 刷新 docker 组
docker run hello-world			# 测试无 root 权限能否使用 docker
```

服务器与容器时区同步问题
```
1. 到容器里查看下时间（时区）：
docker exec -it <容器ID> /bin/bash
$ date
$ exit
2. 用宿主机的时区替换容器的时区：
docker cp /etc/localtime <容器ID>:/etc/localtime
3. 可以再到容器里确认下
```
