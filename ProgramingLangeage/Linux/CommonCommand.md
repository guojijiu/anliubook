###常用命令

####arch
#####说明
查看linux系统的体系结构
#####示例
```
arch
```

fg、bg、jobs、&、ctrl + z都是跟系统任务有关的，虽然现在基本上不怎么需要用到这些命令，但学会了也是很实用的
一。& 最经常被用到
这个用在一个命令的最后，可以把这个命令放到后台执行
二。ctrl + z
可以将一个正在前台执行的命令放到后台，并且暂停
三。jobs
查看当前有多少在后台运行的命令
四。fg
将后台中的命令调至前台继续运行
如果后台中有多个命令，可以用 fg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)
五。bg
将一个在后台暂停的命令，变成继续执行
如果后台中有多个命令，可以用bg %jobnumber将选中的命令调出，%jobnumber是通过jobs命令查到的后台正在执行的命令的序号(不是pid)

### 查看进程所在目录
1.ps -ef|grep redis，得到了进程号 xxxx，

2.ls -l /proc/xxxx/cwd

### 登陆显示用户信息为：bash-4.2
解决：
1. mkdir /home/liuwei
2. chmod 700 /home/liuwei
3. cd /etc/skel/
4. cp .bash_logout /home/liuwei/
5. cp .bash_profile /home/liuwei/
6. cp .bashrc /home/liuwei/

### 服务器问题
问题：
1.安装依赖包报错：Fatal error: Allowed memory size of 2147483648 bytes exhausted (tried to allocate 67108864 bytes) in phar:///usr/local/bin/composer/src/Composer/DependencyResolver/Solver.php on line 223
解决：
增加memory_limit参数
php -d memory_limit=3G /usr/local/bin/composer require --dev zircote/swagger-php -vvv
