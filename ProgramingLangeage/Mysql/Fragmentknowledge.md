### 碎片知识

1. mysql日志类型

```
binlog（归档日志）；
MySQL Server 层记录的日志，所以不管是用的什么存储引擎，只要是 MySQL 都是会有 binlog 的存在，在做 MySQL 主从复制的时候，利用的就是 binlog。
redo log（重做日志）；
InnoDB 存储引擎层方面的日志，所以如果你使用的存储引擎不是 InnoDB 的话，那就根本谈不上 redo log。
undo log（撤消日志）；
```

2. 事务隔离级别

```
读未提交（READ UNCOMMITTED）
读未提交，也叫未提交读，该隔离级别的事务可以看到其他事务中未提交的数据。该隔离级别因为可以读取到其他事务中未提交的数据，而未提交的数据可能会发生回滚，因此我们把该级别读取到的数据称之为脏数据，把这个问题称之为脏读。
读已提交（READ COMMITTED）
读已提交，也叫提交读，该隔离级别的事务能读取到已经提交事务的数据，因此它不会有脏读问题。但由于在事务的执行中可以读取到其他事务提交的结果，所以在不同时间的相同 SQL 查询中，可能会得到不同的结果，这种现象叫做不可重复读。
可重复读（REPEATABLE READ）
可重复读，是 MySQL 的默认事务隔离级别，它能确保同一事务多次查询的结果一致。但也会有新的问题，比如此级别的事务正在执行时，另一个事务成功的插入了某条数据，但因为它每次查询的结果都是一样的，所以会导致查询不到这条数据，自己重复插入时又失败（因为唯一约束的原因）。明明在事务中查询不到这条信息，但自己就是插入不进去，这就叫幻读 （Phantom Read）。
序列化（SERIALIZABLE）
序列化，事务最高隔离级别，它会强制事务排序，使之不会发生冲突，从而解决了脏读、不可重复读和幻读问题，但因为执行效率低，所以真正使用的场景并不多。
```

3. 客户端与服务端的通讯方式

```
1). TCP/IP协议；
    TCP/IP协议是MySQL客户端和服务器最常用的通信方式；
    我们在使用mysql命令启动客户端程序时，只要在-h参数后跟随IP地址作为服务器进程所在的主机地址，那么通讯方式便是TCP/IP协议；
2). UNIX域套接字；
    如果客户端进程和服务器进程都位于类UNIX操作系统（MacOS、Centos、Ubuntu等）的主机之上，并且在启动客户端程序时没有指定主机名，或者指定的主机名为localhost，又或者指定了--protocol=socket的启动参数，那么客户端进程和服务器进程就会使用UNIX域套接字进行进程间通信；
    MySQL服务器进程默认监听的UNIX域套接字文件为/temp/mysql.sock，客户端进程启动时也默认会连接到这个UNIX域套接字文件之上；
3). 命名管道和共享内存；
    如果你的MySQL是安装在Windows主机之上，客户端和服务器进程可以使用命名管道和共享内存的方式进行通信；
```

4. 存储引擎

```
1). 什么是存储引擎？
    把数据存储在什么位置，是内存还是磁盘？怎么从表里读取数据，以及怎么把数据写入具体的表中，这都是存储引擎 负责的事情。
2). 常用的存储引擎？
    InnoDB；
        默认的存储引擎，底层存储结构为 B+树， B 树的每个节点对应 innodb的一个 page，page 大小是固定的，一般设为 16k；
        支持事务，支持外键，因此数据的完整性、一致性更高；
        支持行级别的锁和表级别的锁；
        支持读写并发，写不阻塞读（MVCC）；
        特殊的索引存放方式，可以减少IO，提升査询效率；
    MylSAM：
        支持表级别的锁（插入和更新会锁表），不支持事务；
        拥有较高的插入（insert）和查询（select）速度；
        存储了表的行数（count速度更快）；
    Memory：
        把数据放在内存里面，读写的速度很快，但是数据库重启或者崩溃，数据会全部消失；
        只适合做临时表；
```

5. 为什么使用自增主键，自增主键用完了怎么办？

```
使用自增主键的原因：
    1、数据记录本身被存于主索引的叶子节点上；
    2、mysql会根据其主键将其插入适当的节点和位置；
    3、表使用自增主键，在每次插入新的记录时，记录会顺序添加到当前索引节点的后续位置。
自增主键用完了的解决方案：
    如果表数据大的话，修改表结构就会很慢，从而阻塞更新，删除命令；所以可以采用第三方插件gh-ost，修改自增主键的数据类型；
```

6. 常用函数

```
1. group_concat：把相同名字的code拼接到一起；
2. char_length：获取字符串长度；
3. locate：获取指定内容在某个字符串中的位置；
4. replace：替换字符串的内容；
5. now：当前时间；
6. order by field：自定义排序；
```

7. 常用监控命令：

```
1 连接数（Connects）
最大使用连接数：show status like 'Max_used_connections';
当前打开的连接数：show status like 'Threads_connected';

2 缓存（bufferCache）
未从缓冲池读取的次数：show status like 'Innodb_buffer_pool_reads';
从缓冲池读取的次数：show status like ‘Innodb_buffer_pool_read_requests’
缓冲池的总页数：show status like ‘Innodb_buffer_pool_pages_total’
缓冲池空闲的页数：show status like ‘Innodb_buffer_pool_pages_free’
缓存命中率计算：（1-Innodb_buffer_pool_reads/Innodb_buffer_pool_read_requests）*100%
缓存池使用率为：((Innodb_buffer_pool_pages_total-Innodb_buffer_pool_pages_free）/Innodb_buffer_pool_pages_total）*100%

3 锁（lock）
锁等待个数：show status like ‘Innodb_row_lock_waits’
平均每次锁等待时间：show status like ‘Innodb_row_lock_time_avg’
查看是否存在表锁：show open TABLES where in_use>0；有数据代表存在锁表，空为无表锁

备注：锁等待统计得数量为累加数据，每次获取得时候可以跟之前得数据进行相减，得到当前统计得数据

4 SQL
查看mysql开关是否打开：show variables like ‘slow_query_log’，ON为开启状态，如果为OFF，set global slow_query_log=1 进行开启
查看mysql阈值：show variables like ‘long_query_time’，根据页面传递阈值参数，修改阈值 set global long_query_time=0.1
查看mysql慢sql目录：show variables like ‘slow_query_log_file’
格式化慢sql日志：mysqldumpslow -s at -t 10 /export/data/mysql/log/slow.log
注：此语句通过jdbc执行不了，属于命令行执行。
意思为：显示出耗时最长的10个SQL语句执行信息，10可以修改为TOP个数。显示的信息为：执行次数、平均执行时间、SQL语句

备注：当mysqldumpslow命令执行失败时，将慢日志同步到本地进行格式化处理。

5 statement
insert数量：show status like ‘Com_insert’
delete数量：show status like ‘Com_delete’
update数量：show status like ‘Com_update’
select数量：show status like ‘Com_select’

6 吞吐（Database throughputs）
发送吞吐量：show status like ‘Bytes_sent’
接收吞吐量：show status like ‘Bytes_received’
总吞吐量：Bytes_sent+Bytes_received

7 数据库参数（serverconfig）
show variables
```

8. 慢sql

```
慢SQL指的是MySQL慢查询，具体指运行时间超过long_query_time值的SQL。
我们常听MySQL中有二进制日志binlog、中继日志relaylog、重做回滚日志redolog、undolog等。针对慢查询，还有一种慢查询日志slowlog，用来记录在MySQL中响应时间超过阀值的语句。慢SQL对实际生产业务影响是致命的，所以测试人员在性能测试过程中，对数据库SQL语句执行情况实施监控，给开发提供准确的性能优化意见显得尤为重要。那怎么使用Mysql数据库提供的慢查询日志来监控SQL语句执行情况，找到消耗较高的SQL语句，以下详细说明一下慢查询日志的使用步骤：

确保打开慢SQL开关slow_query_log：show variables like 'slow_query_log'

设置慢SQL域值long_query_time
这个long_query_time是用来定义慢于多少秒的才算“慢查询”，注意单位是秒，我通过执行sql指令set long_query_time=1来设置了long_query_time的值为1, 也就是执行时间超过1秒的都算慢查询：show variables like 'long_query_time'

查看慢SQL日志路径：show variables like 'slow_query_log_file'

通过慢sql分析工具mysqldumpslow格式化分析慢SQL日志
mysqldumpslow慢查询分析工具，是mysql安装后自带的，可以通过./mysqldumpslow —help查看使用参数说明
见用法：
取出使用最多的10条慢查询
./mysqldumpslow -s c -t 10 /export/data/mysql/log/slow.log
取出查询时间最慢的3条慢查询
./mysqldumpslow -s t -t 3 /export/data/mysql/log/slow.log
注意: 使用mysqldumpslow的分析结果不会显示具体完整的sql语句,只会显示sql的组成结构;
假如: SELECT_FROM sms_send WHERE service_id=10 GROUP BY content LIMIT 0, 1000;
mysqldumpslow命令执行后显示：
Count: 2 Time=1.5s (3s) Lock=0.00s (0s) Rows=1000.0 (2000), vgos_dba[vgos_dba]@[10.130.229.196]SELECT_FROM sms_send WHERE service_id=N GROUP BY content LIMIT N, N
mysqldumpslow的分析结果详解：

Count：表示该类型的语句执行次数，上图中表示select语句执行了2次。
Time：表示该类型的语句执行的平均时间（总计时间）
Lock：锁时间0s。
Rows：单次返回的结果数是1000条记录，2次总共返回2000条记录。
通过这个工具就可以查询出来哪些sql语句是慢SQL，从而反馈研发进行优化，比如加索引，该应用的实现方式等。

常见慢SQL排查

不使用子查询
SELECT_FROM t1 WHERE id (SELECT id FROM t2 WHERE name=’hechunyang’);
子查询在MySQL5.5版本里，内部执行计划器是这样执行的：先查外表再匹配内表，而不是先查内表t2，当外表的数据很大时，查询速度会非常慢。
在MariaDB10/MySQL5.6版本里，采用join关联方式对其进行了优化，这条SQL会自动转换为 SELECT t1._FROM t1 JOIN t2 ON t1.id = t2.id;
但请注意的是：优化只针对SELECT有效，对UPDATE/DELETE子 查询无效， 生产环境尽量应避免使用子查询。
避免函数索引
SELECT_FROM t WHERE YEAR(d) >= 2016;
由于MySQL不像Oracle那样⽀持函数索引，即使d字段有索引，也会直接全表扫描。
应改为 > SELECT_FROM t WHERE d >= ‘2016-01-01’;
用IN来替换OR低效查询
慢SELECT_FROM t WHERE LOC_ID = 10 OR LOC_ID = 20 OR LOC_ID = 30;
高效查询 > SELECT_FROM t WHERE LOC_IN IN (10,20,30);
LIKE双百分号无法使用到索引
SELECT_FROM t WHERE name LIKE ‘%de%’;
使用SELECT_FROM t WHERE name LIKE ‘de%’;
分组统计可以禁止排序
SELECT goods_id,count() FROM t GROUP BY goods_id;
默认情况下，MySQL对所有GROUP BY col1，col2…的字段进⾏排序。如果查询包括GROUP BY，想要避免排序结果的消耗，则可以指定ORDER BY NULL禁止排序。
使用SELECT goods_id,count() FROM t GROUP BY goods_id ORDER BY NULL;
禁止不必要的ORDER BY排序
SELECT count(1) FROM user u LEFT JOIN user_info i ON u.id = i.user_id WHERE 1 = 1 ORDER BY u.create_time DESC;
使用SELECT count(1) FROM user u LEFT JOIN user_info i ON u.id = i.user_id;
```
