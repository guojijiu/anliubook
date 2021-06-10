### 编译安装php

1. 前置工作
yum -y install epel-release yum-utils
yum config-manager --set-enabled PowerTools
yum -y install gcc gcc-c++ make autoconf bzip2 bzip2-devel libpng libpng-devel freetype-devel gmp-devel readline-devel curl-devel libxml2-devel libjpeg-devel bison openssl-devel uw-imap-devel libc-client sqlite-devel libicu-devel libedit-devel libxslt-devel oniguruma oniguruma-devel

2. 下载php安装包，从本地下载好后，上传到服务器
wget https://www.php.net/distributions/php-7.4.10.tar.gz

3. tar -zxvf php-7.4.10.tar.gz

4. cd php-7.4.10

5. ./configure --prefix=/usr/local/php --with-config-file-path=/etc --with-fpm-user=deploy --with-fpm-group=deploy --with-curl --with-freetype-dir --enable-gd --with-gettext --with-iconv-dir --with-kerberos --with-libdir=lib64 --with-libxml-dir --with-mysqli --with-openssl --with-pcre-regex --with-pdo-mysql --with-pdo-sqlite --with-pear --with-png-dir --with-jpeg-dir --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --enable-libxml --enable-inline-optimization --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --enable-zip --enable-fpm

6. make && make install

7. # 配置PHP
(1)php -v
/usr/local/php/bin/php -v
vim /etc/profile
PATH=$PATH:/usr/local/php/bin
export PATH
source /etc/profile
php -v
8. 配置PHP-fpm
cp php.ini-production /etc/php.ini
cp sapi/fpm/php-fpm.conf /usr/local/php/etc/php-fpm.conf
cp sapi/fpm/www.conf /usr/local/php/etc/php-fpm.d/www.conf
cp sapi/fpm/php-fpm.service /etc/systemd/system/php-fpm.service
ln -s /usr/local/php/bin/* /usr/local/bin
ln -s /usr/local/php/sbin/* /usr/local/sbin

9. 设置自启动
systemctl start php-fpm
systemctl enable php-fpm

10. 安装redis扩展
wget http://pecl.php.net/get/redis-4.0.2.tgz
tar -zxvf redis-4.0.2.tgz
cd redis-4.0.2
phpize
# whereis php-config  //查看php-config路径
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
进入php.ini添加扩展extension=redis.so
# 重启nginx
    systemctl restart nginx.service
# 重启php-fpm 
    systemctl restart php-fpm.service