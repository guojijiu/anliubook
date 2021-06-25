### 安装zip扩展

yum install -y php-devel #用于编译
cd /usr/local/src
#需要高版本的到官网查询[http://pecl.php.net/package/zip](http://pecl.php.net/package/zip)
wget http://pecl.php.net/get/zip-1.15.3.tgz 
tar -zxvf zip-1.15.3.tgz
cd zip-1.15.3 
phpize
whereis php-config
./configure --with-php-config=/usr/local/bin/php-config

报错：configure: error: Please reinstall the libzip distribution

yum install -y cmake
cd ../
yum remove libzip
wget https://libzip.org/download/libzip-1.2.0.tar.gz
tar -zxvf libzip-1.2.0.tar.gz
cd libzip-1.2.0.tar.gz
mkdir build && cd build && /usr/local/bin/cmake .. && make && make install

报错：CMake Error at CMakeLists.txt:4 (CMAKE_MINIMUM_REQUIRED):
  CMake 3.0.2 or higher is required.  You are running version 2.8.12.2
-- Configuring incomplete, errors occurred!

cmake版本过低，需新版本
yum remove cmake
yum install -y cmake3
cmake -version

安装zip扩展
cd ../zip-1.15.3
./configure --with-php-config=/usr/local/bin/php-config
make
make install

报错：fatal error: zipconf.h: No such file or directory
find /usr/local -iname 'zipconf.h'
ln -s /usr/local/lib/libzip/include/zipconf.h /usr/local/include

再次执行make && make install

修改 /etc/php.ini ：
zlib.output_compression = On
extension=zip.so

systemctl restart php-fpm