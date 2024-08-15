#xdebug介绍
1. Xdebug是PHP的扩展，可帮助进行调试和开发;
2. 它包含一个用于IDE的单步调试器;
3. 它升级了PHP的var_dump（）函数;
4. 它为通知，警告，错误和异常添加堆栈跟踪;
5. 它具有记录每个函数调用和向磁盘分配变量的功能;
6. 它包含一个探查器;
7. 它提供了与PHPUnit一起使用的代码覆盖功能;

#xdebug安装
1. 到[xdebug官网](http://www.xdebug.org/wizard.php)，复制phpinfo()里面的信息，获取对应的xdebug版本进行下载已经按照说明进行安装；
2. 安装完成后，进入/etc/php.ini文件，shift+G，到末尾，添加如下内容
```
windows，php7.4版本
[xdebug]
zend_extension = php_xdebug.dll
xdebug.remote_autostart = 1
xdebug.remote_enable = 1
xdebug.remote_host = "127.0.0.1"
xdebug.remote_port = 9003
xdebug.remote_handler = dbgp
xdebug.start_with_request = yes
xdebug.idekey = PHPSTORM

mac，php7.4版本
[xdebug]
zend_extension="xdebug.so"
xdebug.remote_handler = dbgp
xdebug.idekey = PHPSTORM
xdebug.start_with_request=yes
xdebug.mode=debug
xdebug.client_host=127.0.0.1
xdebug.client_port=9003
```
3. 配置完成后保存，可以用***php -m*** 查看安装好的xdebug，如果要在phpinfo中看到，需要***重启php-fpm***

#xdebug配合phpstrom使用
1. 到Settings->Languages & Frameworks->PHP->Servers，进行设置：Name:(自定义)，Host:192.168.2.139（远程ip），勾选Use path mapping , 在Project file选择项目的根目录，在对应的后面加上映射在远程服务器的根目录，设置完成后点击保存；
2. 点击右上角的Edit Configurations，在弹出的窗口中点击“+”，选择“PHP Remote Debug” , 设置内容，Name:(自定义) , Server:vm.hanju.com(远程服务器的地址) , IDE Key(session id):PHPSTORM(远程服务器上设置的idekey)，完成后点击保存；
3. 开启小电话，在需要打断点的地方进行断点调试即可。

#安装xdebug过程中遇到的问题
```
问题：Cannot accept external Xdebug connection: Cannot evaluate expression 'isset($_SERVER['PHP_IDE_CONFIG'])
解决： For anyone who finds this, the problem was caused by using 'extension' rather than 'zend_extension' to load the xdebug module in the php.ini.
```
```
问题：It may be caused by path mappings misconfiguration or not synchronized local and remote projects.
解决：在Settings->Languages & Frameworks->PHP->Servers，Host:192.168.2.139（远程ip）选择远程ip的时候，不能用户ip，要用域名
```