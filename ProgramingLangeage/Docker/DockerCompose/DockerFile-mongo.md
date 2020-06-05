```
FROM mongo:4
MAINTAINER anliu


RUN  chmod 777 /var/log/mongodb -R \
	&& mkdir /home/mongodb -p \
	&& touch /home/mongodb/.dbshell \
	&& chmod 777 /home/mongodb/.dbshell


#设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
```

