### 安装

1. 下载安装包：http://nginx.org/en/download.html；
2. 安装依赖：um install -y pcre pcre-devel openssl openssl-devel；
3. 添加用户：useradd nginx -s /sbin/nologin -M；
4. 编译安装：
    ./configure
    make &&make install；
5. 增加软链接：ln -s /usr/local/nginx/sbin/nginx /usr/local/bin/；
