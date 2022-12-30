### 编译安装php

# 卸载之前的php
rpm -qa | grep php
php70w-common-7.0.33-1.w7.x86_64
php70w-devel-7.0.33-1.w7.x86_64
php70w-7.0.33-1.w7.x86_64
php70w-cli-7.0.33-1.w7.x86_64
一个一个卸载

rpm -e php70w-7.0.33-1.w7.x86_64
rpm -e php70w-devel-7.0.33-1.w7.x86_64
rpm -e php70w-cli-7.0.33-1.w7.x86_64
rpm -e php70w-common-7.0.33-1.w7.x86_64
  补充一下  如果卸载的时候提示存在依赖关系  

rpm -e --nodeps    价格 --modeps 即可 

# 编译安装

1. 前置工作
yum -y install epel-release yum-utils
yum config-manager --set-enabled PowerTools
yum -y install gcc gcc-c++ make autoconf bzip2 bzip2-devel libpng libpng-devel freetype-devel gmp-devel readline-devel curl-devel libxml2-devel libjpeg-devel bison openssl-devel uw-imap-devel libc-client sqlite-devel libicu-devel libedit-devel libxslt-devel oniguruma oniguruma-devel

2. 下载php安装包，从本地下载好后，上传到服务器
wget https://www.php.net/distributions/php-7.4.10.tar.gz

3. tar -zxvf php-7.4.10.tar.gz

4. cd php-7.4.10

5. ./configure --prefix=/usr/local/php --with-config-file-path=/etc --with-fpm-user=deploy --with-fpm-group=deploy --with-curl --enable-gd --with-jpeg --with-freetype --with-gettext --with-iconv-dir --with-kerberos --with-libdir=lib64  --with-mysqli --with-openssl --enable-pdo --with-pdo-mysql --with-pdo-sqlite --with-pear --with-xmlrpc --with-xsl --with-zlib --with-bz2 --with-mhash --enable-fpm --enable-bcmath --with-libxml --enable-inline-optimization --enable-mbregex --enable-mbstring --enable-opcache --enable-pcntl --enable-shmop --enable-soap --enable-sockets --enable-sysvsem --enable-sysvshm --enable-xml --with-zip

### 报错Alternatively, you may set the environment variables LIBZIP_CFLAGSand LIBZIP_LIBS to avoid the need to call pkg-config.（参考https://www.cnblogs.com/equation/p/12352596.html）
解决方案：
yum remove libzip libzip-devel
wget https://hqidi.com/big/libzip-1.2.0.tar.gz
tar -zxvf libzip-1.2.0.tar.gz
cd libzip-1.2.0
./configure
make && make install
在你configure的会话窗口直接输入如下内容：
export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig/"

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
pecl channel-update pecl.php.net
pecl install redis
或者
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

# 安装composer

1. 下载composer
curl -sS https://getcomposer.org/installer | php
2. 设置环境变量；
mv composer.phar /usr/local/bin/composer
3. 修改权限
chmod -R 777 /usr/local/bin/composer
4. 修改源
composer config -g repo.packagist composer mirrors.aliyun.com/composer/
5. 查看全局配置
composer config -gl
