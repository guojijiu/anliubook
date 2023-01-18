# qsub

### crontab可以不能执行qsub命令，进行投递，但是单独取出命令行，可以执行

```
原因：在脚本中添加所有要用到的环境变量路径等
解决方案：加载qsub的环境变量，source /opt/sge/default/common/settings.sh;
例子：source /opt/sge/default/common/settings.sh; /opt/sge/bin/lx-amd64/qsub -V -cwd -o /share/deploy/taskLog/603/20210510091708/try3qsub/stdout.txt -e /share/deploy/taskLog/603/20210510091708/try3qsub/stderr.txt /share/deploy/taskLog/603/20210510091708/try3qsub/hwc.5649.work.sh
```

### 查看所所有的队列

```
qstat -g c
qstat -Q
```

### qstat -f 查看状态为E

```
解决办法：使用root重启sge
/opt/sge/default/common/sgemaster restart
状态说明：
a： 负载超限了，开启警报alarm。
A： 超限暂替，开启警报Alarm。
E： 队列有错误，不能提供任务提交服务了。
au：主机和SGE系统连接中断，此时负载状态为-NA-。需要重启相应服务器的sgeexecd命令。

* B  只用于任务向量，表示任务向量已经开始执行
* E  任务在运行后退出
* H  任务被服务器或用户或者管理员阻塞
* Q  任务正在排队中，等待被调度运行
* R  任务正在运行
* S  任务被服务器挂起，由于一个更高优先级的任务需要当前任务的资源
* T  任务被转移到其它执行节点了
* U  由于服务器繁忙，任务被挂起
* W  任务在等待它所请求的执行时间的到来(qsub -a)
* X  只用于子任务，表示子任务完成
```

### 执行qstat，报错：error: commlib error: got select error (Connection refused)

```
需要启动服务：
1. /opt/sge/gridengine/default/common;
2. ./sgemaster
```

### qsub执行状态一直为qw

```
管理节点：
1. /opt/sge/gridengine/default/common;
2. ./sgeexecd
3. ./sgemaster

计算节点：
1. /opt/sge/gridengine/default/common;
2. ./sgeexecd
```
