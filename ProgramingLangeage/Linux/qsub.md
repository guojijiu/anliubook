# qsub

### crontab可以不能执行qsub命令，进行投递，但是单独取出命令行，可以执行
原因：在脚本中添加所有要用到的环境变量路径等
解决方案：加载qsub的环境变量，source /opt/sge/default/common/settings.sh;
例子：source /opt/sge/default/common/settings.sh; /opt/sge/bin/lx-amd64/qsub -V -cwd -o /share/deploy/taskLog/603/20210510091708/try3qsub/stdout.txt -e /share/deploy/taskLog/603/20210510091708/try3qsub/stderr.txt /share/deploy/taskLog/603/20210510091708/try3qsub/hwc.5649.work.sh

### 查看所所有的队列
qstat -g c
qstat -Q

### qstat -f 查看状态为E
解决办法：使用root重启sge
/opt/sge/default/common/sgemaster restart
状态说明：
a： 负载超限了，开启警报alarm。
A： 超限暂替，开启警报Alarm。
E： 队列有错误，不能提供任务提交服务了。
au：主机和SGE系统连接中断，此时负载状态为-NA-。需要重启相应服务器的sgeexecd命令。
