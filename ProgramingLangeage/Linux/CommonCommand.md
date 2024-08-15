### 常用命令

#### arch

##### 说明

查看linux系统的体系结构

##### 示例

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

### iotop

哪个进程产生了 IO，数据读取速度等信息，这个时候就需要 iotop 这个工具了;
参数说明：
-o：只显示正在产生 I/O 的进程或线程；
-b：非交互模式，一般用来记录日志；
-n NUM：设置监测的次数，默认无限；
-d SEC：设置每次监测的间隔，默认 1 秒；
-p PID：指定监测的进程/线程；
-u USER：指定监测某个用户产生的 I/O；
-P：仅显示进程，默认 iotop 显示所有线程；
-a：显示累积的 I/O，而不是带宽；
-k：使用 kb 单位进行显示；
-t：时间戳；
-q：只在第一次监测时显示列名；
-qq：将永远不显示列名；
-qqq：将永远不显示 I/O 汇总；

### service --status-all

查看所有服务当前的运行状态。将按照字母的顺序运行所有的 init 脚本。

### chkconfig --list

显示所有运行级系统服务的运行状态信息（on或off）。如果指定了name，那么只显示指定的服务在不同运行级的状态。

### initctl list

initctl 是守护进程控制工具，管理员可以与 Upstart 守护进程进行通信和交互。

### free

用于显示内存状态，会显示内存的使用情况，实体内存，虚拟交换内存，共享内存，以及系统核心使用的缓冲区等等；
语法：free [-bkmotV] [-s <间隔秒数>]
参数：
-b：以 byte 为单位显示内存使用情况；
-k：以 KB 为单位显示内存使用情况；
-m：以 MB 为单位显示内存使用情况；
-o：不显示缓冲区调节列；
-t：显示内存总和列；
-V：显示版本信息；
-s <间隔秒速>：将以动态的形式持续观察内存使用情况；

### 两个加密服务器之间传输文件，报错：ssh: connect to host 12.2.2.2 port 3222: Connection timed out，lost connection

解决方案：
保证两个服务器的文件权限
chmod 600 authorized_keys
chmod 700 ~/.ssh
关闭selinux

### rsync

rsync -azP -v -e 'ssh -p 123' /a/b/ user1@111.22.22.33:/c/d/

### >，<，>>

```
>：重定向输出
<：重定向输入
>>：在文件末尾添加
```

### base常用

```
ctrl-w：删除你键入的最后一个单词；
ctrl-u：可以删除行内光标所在位置之前的内容；
ctrl-b和ctrl-f：可以以单词为单位移动光标；
ctrl-a：可以将光标移至行首；
ctrl-e：可以将光标移至行尾；
ctrl-k：可以删除光标至行尾的所有内容；
ctrl-l：清屏
```

### ~/.ssh/config优化，防止特定网络环境下连接断开、压缩数据、多通道等选项

```
TCPKeepAlive=yes
ServerAliveInterval=15
ServerAliveCountMax=6
Compression=yes
ControlMaster auto
ControlPath /tmp/%r@%h:%p
ControlPersist yes
```

### proc，调试正在出现的问题的时候有时会效果惊人

```
/proc/cpuinfo，/proc/meminfo，/proc/cmdline，/proc/xxx/cwd，/proc/xxx/exe，/proc/xxx/fd/，/proc/xxx/smaps
（这里的 xxx 表示进程的 id 或 pid）
```

### 安装cairosvg报错：src/_imagingtk.c:15:20: 致命错误：Python.h：没有那个文件或目录

```
安装：yum install python3-devel
```

### linux按照cairosvg报错Command "python setup.py egg_info" failed with error co

```
pip3 install --upgrade pip
```

### 添加交换分区

```
1、free -h                   #查看swap的大小
2、dd if=/dev/zero of=/var/swap bs=1024 count=1024000         #使用dd命令创建一个/var/swap文件，大小为1G
3、mkswap  /var/swap                #将文件转换为swap格式
4、swapon /var/swap                #挂载并激活swap分区
5、swapon -s                  #查看当前系统中所有激活的swap分区
6、free -h                  #发现swap已经增大了1G
7、echo '/var/swap  swap  swap  defaults        0 0' >>/etc/fstab && mount -a #永久挂载
8、swapoff /var/swap                #卸载刚才我们新增的swap分区
```

### shell脚本，在同步任务中可以执行，在异步任务中无法执行

```
在shell脚本的首行，增加内容：source ~/.bashrc;
```

### linux pecl 报错Connection to `ssl://pecl.php.net:443' failed:

1. php -r "print_r(openssl_get_cert_locations());"
查看cert.pem的路径，
2. 下载wget http://curl.haxx.se/ca/cacert.pem
3. 将证书替换到cert.pem的路径
```
