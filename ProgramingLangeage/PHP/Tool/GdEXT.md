### 安装gd

由于gd依赖了一些扩展包，所以需要先安装依赖包，依赖包如下：
下载对应的依赖包
wget https://download.imagemagick.org/ImageMagick/download/delegates/
freetype-2.4.0.tar.bz2
jpegsrc.v9.tar.gz
zlib-1.2.8.tar.gz
libpng-1.6.16.tar.gz

1、安装freetype
wget "http://download.savannah.gnu.org/releases/freetype/freetype-2.4.0.tar.bz2"
tar jxvf freetype-2.4.0.tar.bz2
cd freetype-2.4.0
./configure --prefix=/usr/local/freetype --enable-shared
make && make install

2、安装jpegsrc
wget https://download.imagemagick.org/ImageMagick/download/delegates/jpegsrc.v9b.tar.gz
tar zxvf jpegsrc.v9b.tar.gz
cd jpeg-9
./configure --prefix=/usr/local/jpeg --enable-shared
make && make install
./configure 时可能会出现异常，解决如下
/usr/bin/install: cannot create regular file`/usr/local/jpeg/include/jconfig.h’
可以手动创建目录:

mkdir -p /usr/local/jpeg/include \
mkdir -p /usr/local/jpeg/lib \
mkdir -p /usr/local/jpeg/bin \
mkdir -p /usr/local/jpeg/man/man1

3、安装zlib

tar -zxvf zlib-1.2.8.tar.gz
cd zlib-1.2.8
./configure --prefix=/usr/local/zlib

make && make install

4、安装libpng
wget https://download.imagemagick.org/ImageMagick/download/delegates/libpng-1.6.31.tar.gz
tar -zxvf libpng-1.6.31.tar.gz
cd libpng-1.6.31
./configure --prefix=/usr/local/libpng

make && make install

### php7.4以后--with-jpeg-dir 改为--with-jpeg，参考https://www.php.net/manual/zh/migration74.other-changes.php#migration74.other-changes.pkg-config
进入安装包的gd目录
make clean
删除安装过的gd.so文件

./configure \
--enable-gd \
--with-php-config=/usr/local/bin/php-config \
--with-jpeg=/usr/local/jpeg \
--with-freetype=/usr/local/freetype

make && make install

### 检查是否安装完成
ldd /usr/local/php/lib/php/extensions/no-debug-non-zts-20190902/gd.so

