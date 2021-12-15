### 7.4编译安装说明

--with-freetype   打开对freetype字体库的支持 

--with-jpeg   打开对jpeg图片的支持 

--with-webp

--with-xpm

--with-libxml

--disable-rpath    关闭额外的运行库文件 

--enable-bcmath    打开图片大小调整,用到zabbix监控的时候用到了这个模块

--enable-shmop --enable-sysvsem  这样就使得你的PHP系统可以处理相关的IPC函数了。

--enable-inline-optimization  优化线程

--with-curl    打开curl浏览工具的支持 

--with-curlwrappers    运用curl工具打开url流 

--enable-mbregex

--enable-fpm 打上PHP-fpm 补丁后才有这个参数，CGI方式安装的启动程序

--enable-mbstring    多字节，字符串的支持 

--with-mcrypt                    mcrypt算法扩展

--with-mhash                     mhash算法扩展

--enable-gd    打开gd库的支持 

--enable-gd-native-ttf   支持TrueType字符串函数库

--with-openssl      openssl的支持，加密传输https时用到的

--enable-pcntl   freeTDS需要用到的，可能是链接mssql 才用到

--enable-sockets     打开 sockets 支持

--with-xmlrpc    打开xml-rpc的c语言 

--with-zip   打开对zip的支持 

--enable-ftp   打开ftp的支持 

 

--with-bz2    打开对bz2文件的支持        

--without-iconv   关闭iconv函数，字符集间的转换 

--with-ttf     打开freetype1.*的支持，可以不加了 

--with-xsl     打开XSLT 文件支持，扩展了libXML2库 ，需要libxslt软件 

--with-gettext     打开gnu 的gettext 支持，编码库用到 

--with-pear    打开pear命令的支持，PHP扩展用的 

--enable-calendar    打开日历扩展功能

--enable-exif    图片的元数据支持 

--enable-magic-quotes    魔术引用的支持 

 

--disable-debug    关闭调试模式 

--with-mime-magic=/usr/share/file/magic.mime      魔术头文件位置

CGI方式安装才用的参数

 

--enable-fastCGI            支持fastcgi方式启动PHP

--enable-force-CGI-redirect        重定向方式启动PHP

--with-ncurses         支持ncurses 屏幕绘制以及基于文本终端的图形互动功能的动态库

--with-gmp  应该是支持一种规范

--enable-dbase                     建立DBA 作为共享模块

--with-pcre-dir=/usr/local/bin/pcre-config      perl的正则库案安装位置

--disable-dmalloc

--with-gdbm                     dba的gdbm支持

--enable-sigchild

--enable-sysvshm

--enable-zend-multibyte         支持zend的多字节

--enable-wddx

--enable-soap


PHP7.4 一些变更
CURL:
--with-curl no longer accepts a directory.
Enchant:
--with-enchant no longer accepts a directory.
FPM:
--with-fpm-systemd now uses only pkg-config for libsystem checks. The libsystemd minimum required version is 209.
GD:
--with-gd becomes --enable-gd (whether to enable the extension at all) and --with-external-gd (to opt into using an external libgd, rather than the bundled one).
--with-png-dir has been removed. libpng is required.
--with-zlib-dir has been removed. zlib is required.
--with-freetype-dir becomes --with-freetype
--with-jpeg-dir becomes --with-jpeg
--with-webp-dir becomes --with-webp
--with-xpm-dir becomes --with-xpm
IMAP:
--with-kerberos-systemd no longer accepts a directory.
Intl:
--with-icu-dir has been removed. If --enable-intl is passed, then libicu is always required.
LDAP:
--with-ldap-sasl no longer accepts a directory.
Libxml:
--with-libxml-dir has been removed.
--enable-libxml becomes --with-libxml.
--with-libexpat-dir has been renamed to --with-expat and no longer accepts a directory.
Litespeed:
--with-litespeed becomes --enable-litespeed.
Mbstring:
--with-onig has been removed. Unless --disable-mbregex has been passed, libonig is required.
ODBC:
--with-iodbc no longer accepts a directory.
--with-unixODBC without a directory now uses pkg-config (preferred). Directory is still accepted for old versions without libodbc.pc.
OpenSSL:
--with-openssl no longer accepts a directory.
PCRE:
--with-pcre-regex has been removed. Instead --with-external-pcre is provided to opt into using an external PCRE library, rather than the bundled one.
PDO_SQLite:
--with-pdo-sqlite no longer accepts a directory.
Readline:
--with-libedit no longer accepts a directory.
Sodium:
--with-sodium no longer accepts a directory.
SQLite3:
--with-sqlite3 no longer accepts a directory.
XSL:
--with-xsl no longer accepts a directory.
Zip:
--with-libzip has been removed.
--enable-zip becomes --with-zip.
