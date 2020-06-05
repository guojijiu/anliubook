```
FROM golang:1.14
MAINTAINER anliu

RUN apt-get update && apt-get install -y vim
WORKDIR $GOPATH/src
 
EXPOSE 8088

#设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

```
