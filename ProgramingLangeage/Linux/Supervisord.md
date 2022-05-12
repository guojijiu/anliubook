1. 在/etc/supervisord.d，添加cloud_platform_redis.conf
```
[program:cloud_platform_redis]

process_name=%(program_name)s_%(process_num)02d

command=php /var/www/cloud_platform/artisan queue:work --sleep=3 --tries=3
//自动开启 
autostart=true
//自动重启
autorestart=true
//启动进程的用户
user=deploy
//进程数量
numprocs=8
//日志路径，确保路径存在
stdout_logfile=/var/www/cloud_platform/storage/logs/worker.log
```

2.执行以下命令启动进程监控
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl restart cloud_platform_redis:*

3.使用sudo supervisorctl status查看当前进程状态

4.修改内容后执行sudo supervisorctl reload 


# 如果任务无法执行，关掉supervisord，单独启动，再次尝试
1. 通过比对成功的qsub和没有的qsub，确定文件目录未指定，导致无法读取内容，修改参数，directory=，改变配置

### supervisord报错error: <class 'socket.error'>, [Errno 2] No such file or directory: file: /usr/lib64/python2.7/socket.py line: 224
解决：
sudo /usr/bin/python2 /usr/bin/supervisord -c /etc/supervisor/supervisord.conf

### supervisord报错 (no such group)
解决：
在/etc/supervisord.conf，查看配置项是否正确
;[include]
files = /etc/supervisord.conf.d/*.conf

### 常用命令
```
[unix_http_server]
file=/tmp/supervisor.sock   ;UNIX socket 文件，supervisorctl用XML_RPC和supervisord通信就是通过它进行，如果不设置的话，supervisorctl也就不能用了 
;chmod=0700                 ;socket文件的mode(权限)，默认是0700
;chown=nobody:nogroup       ;socket文件的owner(属组)，格式：uid:gid
;username=user              ;使用supervisorctl连接的时候，认证的用户（默认没有用户名）
;password=123               ;和上面的用户名对应的密码，可以直接使用明码，也可以使用SHA加密（默认没有密码）
;[inet_http_server]         ;侦听在TCP上的socket，Web Server和远程的supervisorctl都要用到它不设置的话，默认为不开启。
;port=127.0.0.1:9001        ;这个是管理后台运行的IP和端口，侦听所有IP用:9001或*:9001。如果上面的[inet_http_server]开启了，就必须设置它。如果开放到公网，需要注意安全性
;username=user              ;同上面的[unix_http_server]用户名配置一致
;password=123               ;同上面的[unix_http_server]密码配置一致
 
[supervisord]				 ;这个主要是定义supervisord这个服务端进程的一些参数的
logfile=/tmp/supervisord.log ;这个是supervisord这个主进程的日志路径，注意和子进程的日志没有关系。默认是 $CWD/supervisord.log，$CWD是当前目录。
logfile_maxbytes=50MB        ;日志文件大小，默认50MB。超过50M的时候，会生成一个新的日志文件；如果设成0，表示不限制大小。
logfile_backups=10           ;日志文件保留备份数量默认10，设为0表示不备份
loglevel=info                ;日志级别，默认info，其它: critical, error, warn, info, debug, trace, or blather
pidfile=/tmp/supervisord.pid ;pid 文件
nodaemon=false               ;是否在前台启动，默认是false，即以 daemon 的方式启动
minfds=1024                  ;可以打开的文件描述符的最小值，默认 1024，低于这个值supervisor将不会启动。
minprocs=200                 ;可以打开的进程数的最小值，默认200，低于这个值supervisor也将不会正常启动。
;umask=022                   ;进程创建文件的掩码,默认为022。
;user=chrism                 ; 这个参数可以设置一个非root用户，当我们以root用户启动supervisord之后。我这里面设置的这个用户，也可以对supervisord进行管理,默认情况是不设置。
;identifier=supervisor       ;这个参数是supervisord的标识符，主要是给XML_RPC用的。当你有多个supervisor的时候，而且想调用XML_RPC统一管理，就需要为每个supervisor设置不同的标识符,默认是supervisord。
;directory=/tmp              ;这个参数是当supervisord作为守护进程运行的时候，设置这个参数的话，启动supervisord进程之前，会先切换到这个目录默认不设置。
;nocleanup=true              ;这个参数当为false的时候，会在supervisord进程启动的时候，把以前子进程产生的日志文件(路径为AUTO的情况下)清除掉。有时候咱们想要看历史日志，当 然不想日志被清除了。所以可以设置为true，默认是false，有调试需求的同学可以设置为true。
;childlogdir=/tmp            ;当子进程日志路径为AUTO(系统自动分配)的时候，子进程日志文件的存放路径。
;environment=KEY="value"     ;这个是用来设置环境变量的，supervisord在linux中启动默认继承了linux环境变量，在这里可以设置supervisord进程特有的其他环境变量。supervisord启动子进程时，子进程会拷贝父进程的内存空间内容。 所以设置的这些环境变量也会被子进程继承。默认为不设置。
;strip_ansi=false            ;这个选项如果设置为true，会清除子进程日志中的所有ANSI 序列。什么是ANSI序列呢？就是我们的\n,\t这些东西。默认为false。

[supervisorctl]				 ;这个主要是针对supervisorctl的一些配置  
serverurl=unix:///tmp/supervisor.sock ;通过UNIX socket连接supervisord，路径与unix_http_server部分的file一致
;serverurl=http://127.0.0.1:9001 ; 通过HTTP的方式连接supervisord
;username=chris              ;用户名，默认空
;password=123                ;密码，默认空。
;prompt=mysupervisor         ;输入用户名密码时候的提示，默认supervisor。                   
;history_file=~/.sc_history  ;这个参数和shell中的history类似，我们可以用上下键来查找前面执行过的命令默认是no file的。所以我们想要有这种功能，必须指定一个文件。


;[program:xx]			     ;是被管理的进程配置参数，xx是进程的名称
;command=/opt/apache-tomcat-8.0.35/bin/catalina.sh run  ;程序启动命令;可以带参数,如：/home/test.py -a 'hehe'有一点需要注意的是，我们的command只能是那种在终端运行的进程，不能是守护进程。这个想想也知道了，比如说command=service httpd start。httpd这个进程被linux的service管理了，我们的supervisor再去启动这个命令，这已经不是严格意义的子进程了。
;directory=/tmp                ;进程运行前，会前切换到这个目录
;autostart=true              ;在supervisord启动的时候也自动启动
;startsecs=10         		 ;启动10秒后没有异常退出，就表示进程正常启动了，默认为1秒。
;autorestart=true     		 ;程序退出后自动重启,可选值：[unexpected,true,false]，默认为unexpected，表示进程意外杀死后才重启
这个是设置子进程挂掉后自动重启的情况，如果为false的时候，无论什么情况下，都不会被重新启动，如果为unexpected，只有当进程的退出码不在下面的exitcodes里面定义的退出码的时候，才会被自动重启。当为true的时候，只要子进程挂掉，将会被无条件的重启。                               
;exitcodes=0,2               ;注意和上面的的autorestart=unexpected对应。exitcodes里面的定义的退出码是expected的。
;stopsignal=QUIT             ;进程停止信号，可以为TERM, HUP, INT, QUIT, KILL, USR1, or USR2等信号。默认为TERM 。当用设定的信号去干掉进程，退出码会被认为是expected。    
;stopwaitsecs=10             ;这个是当我们向子进程发送stopsignal信号后，到系统返回信息给supervisord，所等待的最大时间。 超过这个时间，supervisord会向该子进程发送一个强制kill的信号。 默认为10秒。
;startretries=3       		 ;启动失败自动重试次数，默认是3。当超过3次后，supervisor将把此进程的状态置为FAIL
;user=tomcat          		 ;用哪个用户启动进程，默认是root。如果supervisord是root启动，我们在这里设置这个非root用户，可以用来管理该program。
;priority=999         		 ;进程启动优先级，默认999，值小的优先启动
;redirect_stderr=true ;把stderr重定向到stdout，默认false
;stdout_logfile_maxbytes=20MB ;stdout日志文件大小，默认50MB
;stdout_logfile_backups = 20  ;stdout日志文件备份数，默认是10
;stdout_logfile=/opt/apache-tomcat-8.0.35/logs/catalina.out ;进程的stdout的日志路径，可以指定路径，AUTO，none等三个选项。设置为none的话，将没有日志产生。设置为AUTO的话，将随机找一个地方生成日志文件，而且当supervisord重新启动的时候，以前的日志文件会被清空。当 ;redirect_stderr=true的时候，sterr也会写进这个日志文件。需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
;stopasgroup=false    		 ;默认为false,进程被杀死时，是否向这个进程组发送stop信号，包括子进程。主要用于，supervisord管理的子进程，这个子进程本身还有子进程。那么我们如果仅仅干掉supervisord的子进程的话，子进程的子进程有可能会变成孤儿进程。所以咱们可以设置可个选项，把整个该子进程的整个进程组都干掉。 设置为true的话，一般killasgroup也会被设置为true。需要注意的是，该选项发送的是stop。默认为false。
;killasgroup=false     		 ;信号默认为false，不过向进程组发送的是kill信号
;stderr_capture_maxbytes=1MB   ;这个一样，和stdout_capture一样。 默认为0，关闭状态
;stderr_events_enabled=false   ;这个也是一样，默认为false
;environment=A="1",B="2"       ;这个是该子进程的环境变量，和别的子进程是不共享的
;serverurl=AUTO    

;[group:thegroupname]  ;就是给programs(子进程)分组，划分到组里面的program。我们就不用一个一个去操作了，我们可以对组名进行统一的操作。 注意：program被划分到组里面之后，就相当于原来的配置从supervisor的配置文件里消失了。supervisor只会对组进行管理，而不再会对组里面的单个program进行管理了
;programs=progname1,progname2 ;组成员，用逗号分开。
;priority=999                  ;优先级，相对于组和组之间说的。默认999。
 
;包含其它配置文件		;当我们要管理的进程很多的时候，都写在主配置文件就不太合适了，这个时候可以把配置信息写到多个文件中，然后include过来
[include]
files = relative/directory/*.ini    ;可以指定一个或多个以.ini结束的配置文件
```