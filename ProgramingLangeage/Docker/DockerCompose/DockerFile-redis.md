```
FROM redis:5
MAINTAINER anliu

 WORKDIR /data/
 #默认的源太慢，原因就是被我大天朝给墙了，所以换成国内，阿里的
 RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

 RUN apt-get clean

 #不能直接 apt-get install curl ,因为容器里面默认apt的包是空的，所以需要更新到本地
 RUN apt-get update 

 #docker 是基于Ubuntu的，所以里面基本默认都带有apt-get这个工具
 RUN apt-get install -y curl \
     && rm -rf /var/lib/apt/lists/* \
     && mkdir -p  /usr/local/etc/redis/ \ 
     && curl http://download.redis.io/redis-stable/redis.conf > /usr/local/etc/redis/redis.conf
             
#设置时区
 ENV TZ=Asia/Shanghai
 RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#CMD [ "redis-server","/usr/local/etc/redis/redis.conf"]
```
