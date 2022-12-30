### 介绍

### PBS 和 SGE
1. PBS是Protable Batch System的缩写，是一个任务管理系统，最初由NASA的Ames研究中心开发，主要为了提供一个能满足异构计算网络需要的软件包，用于灵活的批处理，特别是满足高性能计算的需要，如集群系统、超级计算机和大规模并行系统。
2. PBS的主要特点有：代码开放，免费获取；支持批处理、交互式作业和串行、多种并行作业，如MPI、 PVM、HPF、MPL；PBS是功能最为齐全, 历史最悠久, 支持最广泛的本地集群调度器之一；
3. PBS的目前包括openPBS, PBS Pro和Torque三个主要分支. 其中OpenPBS是最早的PBS系统, 目前已经没有太多后续开发, PBS pro是PBS的商业版本, 功能最为丰富. Torque是Clustering公司接过了OpenPBS, 并给与后续支持的一个开源版本.
4. 当多个用户使用同一个计算资源时，每个用户用PBS脚本提交自己的任务，由PBS对这些任务进行管理和资源的分配；
5. PBS提供的4条命令用于作业管理：
    1. qsub 命令：用于提交作业脚本；
    2. qstat 命令：用于查询作业状态信息；
    3. qdel 命令：用于删除已提交的作业；
    4. qmgr 命令：用于队列管理；

qsub常用命令
```
qstat -j job-id 查看指定作业状态及具体的报错信息
介绍以下几种状态：
r 正在运行
s 作业停止
dr 作业删除后的一种过度状态
qw 排队等待
hqw 排队等待挂起
Eqw 作业出错终止
qstat -f  查看队列节点状态
介绍以下几种状态：
a 负载超限
A 负载超限
E 队列出错，不能提供任务提交服务了
d 队列主机禁用
au 主机和sge服务连接中断，此时负载状态为-NA-，需要重启相关服务器的sge进程。
u 未知状态
3.节点、队列、用户等配置命令
主机分为管理主机、执行主机、提交主机
qconf -sh 显示管理主机
qconf -ah 主机名 添加管理主机
qconf -mh 主机名 修改管理主机
qconf -sel 显示执行主机
qconf -ae 主机名 添加执行主机
qconf -me 主机名 修改执行主机
qconf -ss 显示提交主机
qconf -as 主机名 添加提交主机
qconf -ms 主机名 修改提交主机
主机组操作：
qconf -shgrpl 查看主机组
qconf -shgrp 主机组 查看某一主机组信息
qconf -ahgrp 主机组 添加主机组
qconf -mhgrp 主机组 修改某一主机组
qconf -dhgrp 主机组 删除主机组
管理用户和操作用户命令
qconf -sm 显示系统管理人员列表
qconf -am 添加管理人员
qconf -dm  删除管理人员
qconf -so 显示操作人员列表
qconf -ao 添加操作人员
qconf -do 删除操作人员
用户配置命令
qconf -sul 查看所有用户组
qconf -su 用户组 查看用户组信息
qconf -au 用户名 用户组 向用户组中添加一个或多个用户
qconf -mu 用户组 修改用户组信息
qconf -du 用户名 用户组 把一个或多个用户从用户组中删除
qconf -dul 用户组 删除用户组
队列配置命令
qconf -sql 查看所有队列
qconf -sq 队列名 查看某一队列名信息
qconf -aq 队列名 添加队列    （通过hostlist与节点创建联系，user_list与用户组及用户创建联系）
qconf -mq 队列名 修改队列
qconf -dq 队列名 删除队列
查看用户可用队列及节点：qselect -U 用户名
查看队列可用节点：qselect -q 队列名
```
