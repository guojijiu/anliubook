###dockerComposer
```
version: '3.2'
services:
    php7.4-fpm:
        build: ./php                                                           #指定build Dockerfile生成镜像
        ports:
            - "9000:9000"
        links:
            - "mysql5.7:mysql5.7"
            - "redis5:redis5"
        #depends_on:            #依赖关系，需要先运行php
        #    - "redis5"
        #    - "mysql5.7"
        volumes:
            - "${PWD}/www:/data:rw"                                             #挂载项目目录下的根目录
            - "${PWD}/php/conf/php.ini:/usr/local/etc/php/php.ini:rw"           #挂载php.ini的配置文件
            - "${PWD}/php/conf/php-fpm.conf:/usr/local/etc/php-fpm.conf:rw"
            - "${PWD}/php/conf/php-fpm.d:/usr/local/etc/php-fpm.d:rw"           #将宿主机上php-fpm的项目根目录配置文件映射到容器中
            - "${PWD}/php/logs/php-fpm:/var/log/php-fpm:rw"                     #挂载日志的配置文件
        container_name: "php7.4-fpm"
    nginx:
        build: ./nginx      #指定build Dockerfile生成镜像
        ports:               #端口映射
            - "80:80"
            - "8080:8080"
            - "443:443"
        depends_on:            #依赖关系，需要先运行php
            - "php7.4-fpm"
        links:
            - "php7.4-fpm:php7.4-fpm"
        volumes:
            - "${PWD}/nginx/conf:/etc/nginx/conf:rw"                #映射配置文件目录
            - "${PWD}/nginx/conf/conf.d:/etc/nginx/conf.d:rw"       #将宿主机上nginx配置文件映射到容器中
            - "${PWD}/nginx/logs:/var/log/nginx:rw"                 #映射配置文件重的日志目录
            - "${PWD}/www:/usr/share/nginx/html:rw"                 #映射网站根目录
        container_name: "nginx"                                         #容器名称
        restart: always                                                 #是否自动重启，默认为no
        command: nginx -g 'daemon off;'                                 #防止在容器里面使用了以后退出导致容器关闭，新版本的默认有这个命令
    mysql5.7:
        build: ./mysql
        command: --default-authentication-plugin=mysql_native_password
        restart: always
        environment:
            MYSQL_ROOT_PASSWORD: 123456
        container_name: mysql5.7
        ports:
            - "3306:3306"
        volumes:
            - "${PWD}/mysql/db:/var/lib/mysql"
            - "${PWD}/mysql/conf/my.cnf:/etc/my.cnf"
    redis5:
        build: ./redis
        ports:
            - 6379:6379
        volumes:
            - "${PWD}/redis/data:/var/lib/redis"
            - "${PWD}/redis/logs:/var/log/redis"
            - "${PWD}/redis/conf:/usr/local/etc/redis"    #redis.conf从官网下载http://www.redis.cn/download.html
        command: redis-server /usr/local/etc/redis/redis.conf
        restart: always
        container_name: redis5
    mongo4:
        build: ./mongo
        ports:
            - 27017:27017
        environment:
            MONGO_INITDB_DATABASE: lwadmin
            MONGO_INITDB_ROOT_USERNAME: anliu
            MONGO_INITDB_ROOT_PASSWORD: dG&UaUpjPo81XMsC
        volumes:
        #    - "${PWD}/mongo/init/:/docker-entrypoint-initdb.d"
            - "${PWD}/mongo/db:/var/lib/mongodb"
            - "${PWD}/mongo/conf/mongod.conf:/etc/mongod.conf" // 需要先连到mongo4的虚拟机内，获取到/etc/mongod.conf的配置文件
            - "${PWD}/mongo/logs:/var/log/mongodb"
        command: mongod -f /etc/mongod.conf
        restart: always
        container_name: mongo4
    rabbitmq3:
        build: ./rabbitmq
        ports:
            - 15672:15672
            - 5672:5672
        environment:
            RABBITMQ_DEFAULT_USER: ${RABBITMQ_USER}
            RABBITMQ_DEFAULT_PASS: ${RABBITMQ_PASS}
        volumes:
            - "${PWD}/rabbitmq/db:/var/lib/rabbitmq"
        restart: always
        #network_mode: "host"
        container_name: rabbitmq3
    golang1.14:
        build: ./golang
        ports:
            - 8088:8088
        volumes:
            - "${PWD}/www/go:/go"
        tty: true
        container_name: golang1.14
```
