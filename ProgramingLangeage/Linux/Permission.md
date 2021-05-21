### 权限

一 Linux用户介绍
1、什么是用户？
用户对硬件资源的操作都需要通过操作系统，比如用户要读取硬盘中的一份关键数据
出于安全考虑，操作系统的开发者们都专门开发了安全机制，要使用操作系统必须事先输入正确的用户名与密
码
这便是用户的由来
2、为何要有用户？或者说我们为何要创建用户？
 主要就是权限问题
1、系统上的每一个进程，都需要一个特定的用户运行，一个用户拥有特定的权限，该用户运行的进程与用户
权限一致
2、通常在公司是使用普通用户管理服务器，因为root权限过大，容易出问题
3、如何查看用户相关信息
[root@aliyum ~]# id # 查看当前用户
uid=0(root) gid=0(root) groups=0(root)
[root@aliyum ~]# whoami # 查看当前用户是谁
root
[root@aliyum ~]# id egon # 查看egon用户
uid=0(root) gid=0(root) groups=0(root)
[root@aliyum ~]# who # 查看所有登录的用户
root pts/2 2020-10-23 15:24 (139.227.12.100)
[root@aliyum ~]#
[root@aliyum ~]# ps aux |grep [s]sh # 每一个进程都有其用户
root 1067 0.0 0.2 112920 4328 ? Ss Feb15 0:04 /usr/sbin/sshd
-D
root 27197 0.0 0.2 154708 5576 ? Ss 15:24 0:00 sshd:
root@pts/2
4、linux系统中用户角色划分
在linux系统中的用户分为管理员用户与其他用户
管理员用户拥有最高权限
其他用户根据管理员的分配拥有不同的权限
​
 需要强调的是：
对于linux系统来说，用户的角色是通过UID和GID识别的；用户系统帐号的名称（如egon）其实给人（管理
员）看的，linux系统能够识别的仅仅是UID和GID这样的数字。
​
 uid与gid
1. UID （User Identify）用户ID，唯一标识一个系统用户的帐号，uid在系统中是唯一的。uid相当于
一个人的身份证，用户名就相当于这个人的名字
2. GID （Group Identify）组ID，如果把一个操作系统当成一家公司，uid相当于这个人的员工号，gid相当于他的部门编号。
​
centos7系统之前约定
uid: 0 由超级用户或具备超级用户权限的用户创建的用户（贫民老百姓，大臣，布衣/bin/bash）
uid: 1~499 系统虚拟用户：UID范围1-499，存在满足文件或服务启动的需要。一般不能登录，只是傀儡
uid: 500-65535 普通用户
​
centos7系统约定：
0 超级管理员，最高权限，有着极强的破坏能力
1~200 系统用户，用来运行系统自带的进程，默认已创建
201~999 系统用户，用来运行安装的程序，所以此类用户无需登录系统
1000+ 普通用户，正常可以登录系统的用户，权限比较小，能执行的任务有限
​
 用户和组的关系：
一对一，多对一，一对多，多对多
5、超级用户
默认是root用户，其UID和GID均为0。root用户在每台unix/linux操作系统中都是唯一且真实存在的，通
过它可以登录系统，可以操作系统中任何文件和命令，拥有最高的管理权限。
​
举个例子：
- 1、操作系统=》一个国家
- 2、root用户=》国王
- 3、root用户的家目录=》皇宫
​
需要注意的是：
- 1、在生产环境中，一般会禁止root帐号通过SSH远程连接服务器（保护好皇帝），当然了，也会更改默认
的SSH端口（保护好皇宫），以加强系统安全。
- 2、企业工作中：没有特殊需求，应该尽量不要登录root用户进行操作，应该在普通用户下操作任务，然后
用sudo管理普通用户的权限，可以细到每个命令权限分配。
- 3、在linux系统中，uid为0的用户就是超级用户。但是通常不这么做，如果确实有必要在某一操作上用到
管理的权限的话，那就用sudo单独授权，也不要直接用uid为0的用户
6、扩展阅读
 Linux/Unix是一个多用户、多任务的操作系统
 windows是一个单用户多任务操作系统
​
多用户不是说可以创建多个用户，而是指一次可以登录多个用户
多任务指的是可以并发执行多个进程
​
回忆之前讲过的linux发展史：
multics-》unix-》linux，所以linux是多用户的，天然支持多个连机终端，连机终端在没有互联网的情
况下是有意义的，多个人可以用不同的连机终端连到一台机器/服务器上使用，而有了互联网之后，多个人可
通过网络访问服务器，这个时候多用户or单用户的概念就不再那么重要
二 用户与组相关文件
和用户、组相关的文件：
    /etc/passwd
    /etc/shadow
    /etc/group
    /etc/gshadow
/etc/passwd


root:x:0:0:root:/root:/bin/bash
第一字段：用户名（也被称为登录名)；
第二字段：口令；在例子中我们看到的是一个x，其实密码已被映射到/etc/shadow 文件中；
第三字段：UID ；请参看本文的UID的解说；
第四字段：GID；请参看本文的GID的解说；
第五字段：描述信息，可选
第六字段：用户的家目录所在位置；
第七字段：用户所用SHELL的类型
/etc/shadow


small_egon:$1$VE.Mq2Xf$2c9Qi7EQ9JP8GKF8gH7PB1:13072:0:99999:7:::
big_egon:$1$IPDvUhXP$8R6J/VtPXvLyXxhLWPrnt/:13072:0:99999:7::13108:
​
第一字段：用户名（也被称为登录名)，在/etc/shadow中，用户名和/etc/passwd 是相同的，这样就把
passwd 和shadow中用的用户记录联系在一起；这个字段是非空的；
​
第二字段：密码（已被加密)，如果是有些用户在这段是x，表示这个用户不能登录到系统；这个字段是非空
的；
​
第三字段：上次修改口令的时间；这个时间是从1970年01月01日算起到最近一次修改口令的时间间隔（天
数)，您可以通过passwd 来修改用户的密码，然后查看/etc/shadow中此字段的变化；
​
第四字段：两次修改口令间隔最少的天数；如果设置为0,则禁用此功能；也就是说用户必须经过多少天才能修
改其口令；此项功能用处不是太大；默认值是通过/etc/login.defs文件定义中获取，PASS_MIN_DAYS
中有定义；
​
第五字段：两次修改口令间隔最多的天数；这个能增强管理员管理用户口令的时效性，应该说在增强了系统的
安全性；如果是系统默认值，是在添加用户时由/etc/login.defs文件定义中获取，在PASS_MAX_DAYS
中定义；
​
第六字段：提前多少天警告用户口令将过期；当用户登录系统后，系统登录程序提醒用户口令将要作废；如果
是系统默认值，是在添加用户时由/etc/login.defs文件定义中获取，在PASS_WARN_AGE 中定义；
​
第七字段：在口令过期之后多少天禁用此用户；此字段表示用户口令作废多少天后，系统会禁用此用户，也就
是说系统会不能再让此用户登录，也不会提示用户过期，是完全禁用；
​
第八字段：用户过期日期；此字段指定了用户作废的天数（从1970年的1月1日开始的天数)，如果这个字段的
值为空，帐号永久可用； www.hackdig.com
​
第九字段：保留字段，目前为空，以备将来Linux发展之用；
​
如果更为详细的，请用 man shadow来查看帮助，您会得到更为详尽的资料；
/etc/group：组文件


/etc/gshadow：组密码文件


/etc/skel/ 用户老家的模板
/home/xxx 用户家目录
/var/spool/mail/xxx 用户邮箱文件
三 用户管理命令
用户管理命令汇总
    useradd #添加用户
    userdel #删除用户
    usermod #修改用户信息
1、创建用户
[root@localhost ~]# useradd user1
2、查看用户
[root@localhost ~]# id user1
uid=1002(user1) gid=1003(user1) 组=1003(user1)
[root@localhost ~]# who # 查看所有登录的用户信息
[root@localhost ~]# whoami # 查看当前登录的用户名
​
 注意：当创建一个用户时，如果没有指定用户的主组，将会创建一个同名的组作为用户的主组。
3、删除用户
[root@localhost ~]# userdel user1 # 删除用户user1，但不删除用户家目录和mail
[root@localhost ~]# userdel -r user1 # 要想删彻底,加-r选项
4、useradd命令详解：创建用户的同时指定选项
#怎样在Linux系统中添加一个新的用户账户
1) 掌握useradd命令的功能：新增一个用户。
2) 了解useradd命令的常用选项：
3) –u：指定用户的UID
4) –g：指定用户所属的主群
–G：指定用户所属的附加群
5) –d：指定用户的家目录
6) –c：指定用户的备注信息
7) –s：指定用户所用的shell
8) -e：修改过期时间
9) -M: 不创建家目录
10) -r: 创建系统账户，uid处于系统用户范围内，默认就没有家目录
​
#灵活应用useradd命令的举例：
a) 例如：在系统中新增一个fox（狐狸）用户的命令：useradd fox
b) 例如：在系统中新增一个用户user01，属组为police以及uid为600的命令：
useradd –u 600 –g police user01
 其他练习
[root@root ~]# useradd user01
[root@root ~]# useradd user02 -u 503 # 创建用户usr02，指定uid
[root@root ~]# useradd user03 -d /aaa # 创建用户user03 指定家目录
[root@root ~]# useradd user04 -M # 创建用户user04，不创建家目录
[root@root ~]# useradd user05 -s /sbin/nologin # 创建用户并指定shell
[root@root ~]# useradd user06 -g hr # 创建用户，指定主组
[root@root ~]# useradd user07 -G sale # 创建用户，指定附加组
[root@root ~]# useradd user08 -e 2014-04-01 # 指定过期时间
[root@root ~]# useradd user10 -u 4000 -s /sbin/nologin
[root@aliyum ~]# useradd xxx -M -s /sbin/nologin # 创建普通用户，但是没有家目录，不能
登录系统
[root@aliyum ~]# useradd -r yyy -s /sbin/nologin # yyy属于系统用户，uid处于系统用户
uid范围内
5、usermod命令
同useradd参数基一致，只不过useradd是添加，usermod是修改
-u #指定要修改用户的UID
-g #指定要修改用户基本组
-a #将用户添加到补充组。仅与-G选项一起使用
-G #指定要修改用户附加组，使用逗号隔开多个附加组, 覆盖原有的附加组
-d #指定要修改用户家目录
-c #指定要修改用户注释信息
-s #指定要修改用户的bash shell
​
[root@root ~]# usermod -e 2013-02-11 user1000 # 修改过期时间
[root@root ~]# usermod -g group1 jj # 修改主组
[root@root ~]# usermod -a -G group2 jj # 修改附加组，-a添加，不加-a代表覆盖
​
其他选项
-m #将用户主目录的内容移动到新位置。如果当前主目录不存在，则不会创建新的主目录
-l #指定要修改用户的登陆名
-L #指定要锁定的用户
-U #指定要解锁的用户
6、设定与修改密码
passwd # 默认给当前用户设定密码
passwd 用户名 # root用户可以给自己以及所有其他用户设定密码，普通用户只能设定自己的密码
echo "密码" | passwd --stdin 用户名 # 非交互式
 补充：可以利用系统内置变量生成随机字符串来充当密码
[root@aliyum ~]# echo $RANDOM|md5sum|cut -c 1-10
70ba11a74b
[root@aliyum ~]#
​
 ＝＝修改UID，SHELL＝＝
[root@root ~]# usermod --help
[root@root ~]# useradd user10
[root@root ~]# grep 'user10' /etc/passwd
user10:x:509:509::/home/user10:/bin/bash
[root@root ~]# usermod -u 2000 user10 //修改用户uid
[root@root ~]# usermod -s /sbin/nologin user10 //修改用户shell
​
 补充,-G在没有-a的情况下代表覆盖原有组
[root@root ~]# usermod -G group1,group2 user10 //把用户user10添加到组group1
group2
[root@root ~]# usermod -G group3,group4 user10 //把用户user10添加到组group3
#group4中,此时group3,group4会覆盖掉之前添加的group1,group2
​
 ==密码锁定，解锁＝＝
[root@root ~]# useradd user1000
[root@root ~]# passwd user1000
[root@root ~]# grep 'user1000' /etc/shadow
user1000:$1$Hw2wCJoe$FU91eSBsBx1W0CGdIhTwh/:15775:0:99999:7:::
[root@root ~]# usermod -L user1000 # 锁住用户
[root@root ~]# grep 'user1000' /etc/shadow
user1000:$6$WLHLI8zV$0jVjYsQig.jPNrcikI.R8GsgXPaSnlzslEmsRV1Nyb7FUr9Ls6RosWtHsf3
LPio7PgM80raOqlWzd/lGn5fN71:18484:0:99999:7:::
​
 登录测试
[root@root ~]# usermod -U user1000 # 解锁用户
[root@root ~]# grep 'user1000' /etc/shadow
user1000:$6$WLHLI8zV$0jVjYsQig.jPNrcikI.R8GsgXPaSnlzslEmsRV1Nyb7FUr9Ls6RosWtHsf3
LPio7PgM80raOqlWzd/lGn5fN71:18484:0:99999:7:::
 登录测试
​
 ＝＝设置账号过期＝＝
[root@root ~]# date
Mon Aug 10 22:49:12 CST 2020
[root@root ~]# usermod -e 2020-11-11 user1000
[root@root ~]# grep 'user1000' /etc/shadow
user1000:$6$WLHLI8zV$0jVjYsQig.jPNrcikI.R8GsgXPaSnlzslEmsRV1Nyb7FUr9Ls6RosWtHsf3
LPio7PgM80raOqlWzd/lGn5fN71:18484:0:99999:7::18577:
 登录测试
​
 ==useradd 命令参考的文件==
1. /etc/login.defs
2. /etc/default/useradd # 可以使用命令 useradd -D查看
3. /etc/skel/* # 用户的初始配置文件
扩展阅读
useradd创建用户时，对于未指定的选项（-u、-g等等），会
以/etc/login.defs、/etc/default/useradd两个配置文件中的配置作为参照物
​
#配置文件/etc/login.defs详解
[root@egon ~]# grep -Ev "^#|^$" /etc/login.defs
MAIL_DIR /var/spool/mail
PASS_MAX_DAYS 99999 #密码最大有效期
PASS_MIN_DAYS 0 #两次修改密码的最小间隔时间
PASS_MIN_LEN 5 #密码最小长度，对于root无效
PASS_WARN_AGE 7 #密码过期前多少天开始提示
UID_MIN 1000 #用户ID的最小值
UID_MAX 60000 #用户ID的最大值
SYS_UID_MIN 201 #系统用户ID的最小值
SYS_UID_MAX 999 #系统用户ID的最大值
GID_MIN 1000 #组ID的最小值
GID_MAX 60000 #组ID的最大值
SYS_GID_MIN 201 #系统用户组ID的最小值
SYS_GID_MAX 999 #系统用户组ID的最大值
CREATE_HOME yes #使用useradd的时候是可以创建用户家目录
UMASK 077 #创建家目录时umask的默认控制权限
USERGROUPS_ENAB yes #删除用户的时候是否同时删除用户组
ENCRYPT_METHOD SHA512 #密码加密规则
​
#配置文件/etc/default/useradd详解
[root@egon ~]# cat /etc/default/useradd
GROUP=100 #依赖于/etc/login.defs的USERGRUUPS_ENAB参数，如果为no，则在
此处控制
HOME=/home #把用户的家目录建在/home中。
INACTIVE=-1 #是否启用账号过期停权,-1表示不启用。
EXPIRE= #账号终止日期,不设置表示不启用。
SHELL=/bin/bash #新用户默认所有的shell类型。
SKEL=/etc/skel #配置新用户家目录的默认文件存放路径。
CREATE_MAIL_SPOOL=yes #创建mail文件。
​
当使用useradd创建用户时，创建的用户家目录下会存在.bash_* 环境变量相关的文件，这些环境变量文件
默认
从/etc/skel目录中拷贝。这个默认拷贝环境变量位置是由/etc/default/useradd配置文件中定义的。
​
#故障案例，在当前用户家目录下执行了rm -rf .*命令，下次登录系统时出现-bash-4.1$，如何解决！
-bash-4.1$ cp -a /etc/skel/.bash* ./
-bash-4.1$ exit
[root@egon ~]# #重新连接即可恢复
四 组管理
组管理命令汇总
    groupadd
    groupmod
    groupdel
    gpasswd # 设置组密码
    newgrp # 切换主组
创建组
[root@aliyum ~]# groupadd gg1 #创建基本组, 不指定gid
[root@aliyum ~]# tail -1 /etc/group
gg1:x:2005:
[root@aliyum ~]# groupadd -g 5555 gg2 #创建基本组, 指定gid为5555
[root@aliyum ~]# tail -1 /etc/group
gg2:x:5555:
[root@aliyum ~]# groupadd -r gg3 # 创建系统组，gid从201-999
[root@aliyum ~]# tail -1 /etc/group
gg3:x:991:
[root@aliyum ~]#
修改组
[root@aliyum ~]# groupmod -g 1111 gg3
[root@aliyum ~]# tail -1 /etc/group
gg3:x:1111:
[root@aliyum ~]#
​
[root@aliyum ~]# groupmod -n new_gg3 gg3 # -n 修改组名称
[root@aliyum ~]# tail -1 /etc/group
new_gg3:x:1111:
[root@aliyum ~]#
删除组
 如果一个组是一个用户的主组，那么该组不能被删除，删掉用户会默认一起删掉他的主组
[root@aliyum ~]# useradd egon1
[root@aliyum ~]# groupadd devops
[root@aliyum ~]# usermod -G devops egon1
[root@aliyum ~]# id egon1
uid=2004(egon1) gid=2004(egon1) groups=2004(egon1),5556(devops)
[root@aliyum ~]#
[root@aliyum ~]#
[root@aliyum ~]# groupdel devops # 附加组可以删除
[root@aliyum ~]# id egon1 # 查看用户，发现他的附加组没有了
uid=2004(egon1) gid=2004(egon1) groups=2004(egon1)
[root@aliyum ~]# groupdel egon1 # 无法删除组egon1，因为组egon1属于egon1用户的主组
groupdel: cannot remove the primary group of user 'egon1'
[root@aliyum ~]#
组成员管理
对于用户来说，组是分类的
1、一类是基本组或称主组，用户只能有一个基本组，创建时可通过-g指定，如未指定则创建一个默认
的组(与用户同名)
2、附加组，基本组不能满足授权要求，创建附加组，将用户加入该组，用户可以属于多个附加组，加
入一个组后就拥有了该组的权限
​
注意：gpasswd将用户添加到组或从组中删除，只针对已存在的用户
[root@root ~]# gpasswd -a user07 it # 将某个用户加入到某个组
[root@root ~]# gpasswd -M user02,user03,user04 it # 将多个用户加入到it组
[root@root ~]# gpasswd -A lhf it # 指定lhf为组it的组长，除了root用户外lhf用户也可以采用
第一条命令往it组里添加成员
[root@root ~]# grep 'it' /etc/group # 查看it组中的成员
it:x:505:user02,user03,user04
[root@root ~]# gpasswd -d user07 it # 删除用户usr07从it组
我们可以为组设置密码，然后让一些非组成员的用户通过命令”newgrp 组”临时切换到组内并输入密码
的方式获取用户组的权限和特性，如下
[root@localhost ~]# groupadd group1
[root@localhost ~]# gpasswd group1
正在修改 group1 组的密码
新密码：
请重新输入新密码：
[root@localhost ~]# touch /tmp/a.txt
[root@localhost ~]# ll /tmp/a.txt
-rw-r--r-- 1 root root 0 8月 10 21:01 /tmp/a.txt
[root@localhost ~]# chown .group1 /tmp/a.txt
[root@localhost ~]# !ll
ll /tmp/a.txt
-rw-r--r-- 1 root group1 0 8月 10 21:01 /tmp/a.txt
[root@localhost ~]# chmod g+w /tmp/a.txt
[root@localhost ~]# gpasswd group1
​
[root@localhost ~]# su - egon
上一次登录：一 8月 10 21:01:46 CST 2020pts/0 上
[egon@localhost ~]$ echo 123 >> /tmp/a.txt # 此时没有权限
-bash: /tmp/a.txt: 权限不够
[egon@localhost ~]$ newgrp group1 # 临时切换到组group1下，拥有其权限
密码：
[egon@localhost ~]$ echo 123 >> /tmp/a.txt
[egon@localhost ~]$ cat /tmp/a.txt
123
五 手动创建用户
1、/etc/passwd
[root@aliyun ~]# vim /etc/passwd # 新加一行
[root@aliyun ~]# tail -1 /etc/passwd
egon:x:2002:2002:哈哈哈:/home/egon:/bin/bash
2、/etc/shadow
[root@aliyun ~]# openssl passwd -1 -salt 'i have a dream'
Password:
$1$i have a$jBGkkhpFu9WPSI1Nv.whT/
​
[root@aliyun ~]# vim /etc/shadow
[root@aliyun ~]# tail -1 /etc/shadow
egon:$1$i have a$jBGkkhpFu9WPSI1Nv.whT/:18303::::::
制作密码
openssl passwd 手动生成密码
引言:在Linux系统中我们要向手动生成一个密码可以采用opensll passwd来生成一个密码作为用户账号的
密码。Linux系统中的密码存放在/etc/shadow文件中，并且是以加密的方式存放的，根据加密方式的不
同，所产生的加密后的密码的位数也不同。
​
openssl passwd的作用是用来计算密码hash的，目的是为了防止密码以明文的形式出现。
​
语法格式： openssl passwd [option] passwd
​
openssl passwd常用的选项如下：
​
-1：表示采用的是MD5加密算法。
​
-salt：指定salt值，不使用随机产生的salt。在使用加密算法进行加密时，即使密码一样，salt不一样，
所计算出来的hash值也不一样，除非密码一样，salt值也一样，计算出来的hash值才一样。salt为8字节的
字符串。
​
示例：
[tom@localhost ~]$ openssl passwd -1 -salt 'i have a dream' ##注意'i have a
dream' 不是密码而是密码的盐,注意密码的盐里不要有中文
Password: ##这里输入的是密码
$1$12345678$1qWiC4czIc07B4J8bPjfC0 ##这是生成的密文密码
​
##将生成的密码串，手动添加到/etc/shadow中就可用作用户的登陆密码了。
3、/etc/group
[root@aliyun ~]# vim /etc/group
[root@aliyun ~]# tail -1 /etc/group
egon:x:2002:
4、/etc/gshadow
[root@aliyun ~]# vim /etc/gshadow
[root@aliyun ~]# tail -1 /etc/gshadow
egon:!::
5、创建用户家目录，并用用户老家的模板/etc/skel/ 装修一下，注意权限
[root@aliyun ~]# mkdir /home/egon
[root@aliyun ~]# cp -r /etc/skel/.[!.]* /home/egon/
​
[root@aliyun ~]# chmod 700 /home/egon/
[root@aliyun ~]# chown -R egon.egon /home/egon/
6、/var/spool/mail/xxx 用户邮箱文件
[root@aliyun ~]# touch /var/spool/mail/egon
[root@aliyun ~]# chmod 660 !$
chmod 660 /var/spool/mail/egon
[root@aliyun ~]# chown egon.mail /var/spool/mail/egon
测试账号的登录
[root@aliyun ~]# ssh egon@127.0.0.1
egon@127.0.0.1's password:
Last login: Mon Aug 10 23:18:55 2020 from 127.0.0.1
​
Welcome to Alibaba Cloud Elastic Compute Service !
​
[egon@aliyun ~]$ whoami
egon