### 安装

1. 下载安装包：http://nginx.org/en/download.html；
2. 安装依赖：yum install -y pcre pcre-devel openssl openssl-devel；
3. 添加用户：useradd nginx -s /sbin/nologin -M；
4. 编译安装：
    ./configure
    make &&make install；
5. 设置systemctl控制：
    1). vim /usr/lib/systemd/system/nginx.service；
    2). 添加内容nginx.service；
    3). 执行systemctl daemon-reexec；
    4). 启动systemctl start nginx.service；
    5). 设置为开机自启：systemctl enable nginx；
    6). 查看是否设置开机自启：systemctl is-enabled nginx；

### nginx.service
```
[Unit]
Description=nginx - high performance web server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx.pid
ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
ExecQuit=/usr/local/nginx/sbin/nginx -s quit

[Install]
WantedBy=multi-user.target
```

可选项：
    增加软链接：ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/；