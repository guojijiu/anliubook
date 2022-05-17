###dockerComposer
```
version: '3.2'
services:
    php7.4-fpm:
        build: ./php/7.4                                                          #指定build Dockerfile生成镜像
        ports:
            - "9900:9000"
        volumes:
            - "./www:/data:rw"                                             #挂载项目目录下的根目录
            - "./php/7.4/conf/php.ini:/usr/local/etc/php/php.ini:rw"           #挂载php.ini的配置文件
            - "./php/7.4/conf/php-fpm.conf:/usr/local/etc/php-fpm.conf:rw"
            - "./php/7.4/conf/php-fpm.d:/usr/local/etc/php-fpm.d:rw"           #将宿主机上php-fpm的项目根目录配置文件映射到容器中
            - "./php/7.4/logs/php-fpm:/var/log/php-fpm:rw"                     #挂载日志的配置文件
#            - "/share:/share:rw"
#            - "/opt:/opt:rw"
#            - "/hpcdata:/hpcdata:rw"
        container_name: "php7.4-fpm"
    mysql5.7:
        image: mysql:5.7
        command: '--default-authentication-plugin=mysql_native_password'
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: "M54U%cDmism#Bjjp"
        container_name: mysql5.7
        ports:
            - "3307:3306"
        volumes:
            - "./mysql/5.7/db:/var/lib/mysql"
            - "./mysql/5.7/conf/:/etc/mysql/conf.d"
    mysql-8:
        image: mysql:8
        command: '--default-authentication-plugin=mysql_native_password'
        restart: always
        environment:
            TZ: 'Asia/Shanghai'
            MYSQL_ROOT_PASSWORD: ${MYSQL_PASSWD}
        container_name: mysql8
        ports:
            - "3308:3306"
        volumes:
            - "./mysql/8.0/db:/var/lib/mysql"
            - "./mysql/8.0/conf/:/etc/mysql/conf.d"
    redis5:
        build: ./redis/5.0
        ports:
            - 6375:6379
        volumes:
            - "./redis/5.0/data:/data"
            - "./redis/5.0/logs:/var/log/redis" # 修改/redis/5.0/logs的权限为可读可写
            - "./redis/5.0/conf:/usr/local/etc/redis"    #redis.conf从官网下载http://www.redis.cn/download.html
        command: redis-server /usr/local/etc/redis/redis.conf
        restart: always
        container_name: redis5
    mongo:
        image: mongo:latest
        ports:
            - 17017:27017
        environment:
            MONGO_INITDB_ROOT_USERNAME: admin
            MONGO_INITDB_ROOT_PASSWORD: dG&UaUpjPo81XMsC
            TZ: 'Asia/Shanghai'
        volumes:
            - "./mongo/conf/mongod.conf:/etc/mongod.conf:rw"
            - "./mongo/db:/data/db/:rw"
            #- "./mongo/logs:/var/log/mongodb/:rw"
        command: mongod -f /etc/mongod.conf
        restart: always
        container_name: mongo
```
