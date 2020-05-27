FROM php:7.4-fpm
MAINTAINER anliu "www.anliu.com"

# 设置时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 更新安装依赖包和PHP核心拓展
RUN apt-get update && apt-get install -y \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        librabbitmq-dev \
    && docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && rm -r /var/lib/apt/lists/*

# 将预先下载好的拓展包从宿主机拷贝进去
COPY ./pkg/redis-5.2.1.tgz /home/redis.tgz
COPY ./pkg/xdebug-2.9.4.tgz /home/xdebug.tgz
COPY ./pkg/swoole-4.5.0.tgz /home/swoole.tgz
COPY ./pkg/mongodb-1.7.4.tgz /home/mongodb.tgz

# 安装 PECL 拓展，这里我们安装的是Redis
RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini
RUN pecl install /home/xdebug.tgz && echo "zend_extension=xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini
RUN pecl install /home/swoole.tgz && echo "extension=swoole.so" > /usr/local/etc/php/conf.d/swoole.ini
RUN pecl install /home/mongodb.tgz && echo "extension=mongodb.so" > /usr/local/etc/php/conf.d/mongodb.ini
RUN pecl install amqp 1.10.0
# 安装第三方拓展，这里是 Phalcon 拓展
#RUN cd /home \
#    && tar -zxvf cphalcon.tar.gz \
#    && mv cphalcon-* phalcon \
#    && cd phalcon/build \
#    && ./install \
#    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini

# 安装 Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN rm -f /home/redis.tgz \
        rm -f /home/xdebug.tgz \
        rm -f /home/swoole.tgz \
        rm -f /home/mongodb.tgz

WORKDIR /data

# Write Permission
RUN usermod -u 1000 www-data
