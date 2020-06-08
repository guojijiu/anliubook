###FRP

###目的
1. 实现内网穿透；
2. 利用处于内网或防火墙后的机器，对外网环境提供 http 或 https 服务。

###github地址
源码及文档地址：[https://github.com/fatedier/frp](https://github.com/fatedier/frp)

客户端和服务端下载地址：[https://github.com/fatedier/frp/releases](https://github.com/fatedier/frp/releases)

###配置
####服务端配置：
```
[common]
bind_port = 7000
vhost_http_port = 8188
subdomain_host = 服务器所在的域名
# 配置 dashboard（可选）
dashboard_port = 7500
dashboard_user = root
dashboard_pwd = 123456

```
####客户端配置：
```
[common]
server_addr = 服务器所在的域名
server_port = 7000

[demo]
type = http
local_port = 8188
subdomain = demo
```
使用：

前提：进入到安装目录

服务端：./frps -c frps.ini

客户端：./frpc -c frpc.ini

###基于Linux：
####nginx配置：
```
server {
    listen    8188;
    server_name 服务器所在的域名;
    index  index.html index.htm;

    #配置反响代理
    location / {
	    proxy_pass   http://127.0.0.1:7000;
    }
}
```
####基于docker：
####nginx配置：
```
server {
    listen    8188;
    server_name 服务器所在的域名;
    index  index.html index.htm;

    #配置反响代理
    location / {
	    proxy_pass   http://172.17.0.4:7000;#因为是指向的docker中配置的nginx，指向IP就不是127.0.0.1，而是对应的虚拟IP
    }
}
```
