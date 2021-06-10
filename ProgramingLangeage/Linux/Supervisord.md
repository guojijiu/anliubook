1. 在/etc/supervisord.d，添加cloud_platform_redis.conf
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

2.执行以下命令启动进程监控
sudo supervisorctl reread

sudo supervisorctl update

sudo supervisorctl restart cloud_platform_redis:*

3.使用sudo supervisorctl status查看当前进程状态

4.修改内容后执行sudo supervisorctl reload 


### supervisord报错error: <class 'socket.error'>, [Errno 2] No such file or directory: file: /usr/lib64/python2.7/socket.py line: 224
解决：
sudo /usr/bin/python2 /usr/bin/supervisord -c /etc/supervisor/supervisord.conf

### supervisord报错 (no such group)
解决：
在/etc/supervisord.conf，查看配置项是否正确
;[include]
files = /etc/supervisord.conf.d/*.conf

