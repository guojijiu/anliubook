### 安装

1. 下载mysql：wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz；
2. 解压 mysql：tar xvf mysql-8.0.20-linux-glibc2.12-x86_64.tar.xz；
3. 重命名：mv mysql-8.0.20-linux-glibc2.12-x86_64 mysql；
4. 移动目录：mv mysql /usr/local；
5. 切换到mysql文件夹下：cd mysql；
6. 创建data目录：mkdir data；
7. 创建用户组以及用户和密码：
    groupadd mysql
    useradd -g mysql mysql
8. 授权用户：chown -R mysql.mysql /usr/local/mysql；
9. 切换到bin目录下：cd bin；
10. 初始化基础信息：
    ./mysqld --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data/ --initialize
11. 编辑my.cnf文件：vim /etc/my.cnf；
12. 修改信息：
    [mysqld]
    basedir=/usr/local/mysql/ 
    datadir=/usr/local/mysql/data/
    socket=/tmp/mysql.sock
    character-set-server=UTF8MB4
13. 进入/usr/local/mysql，添加mysql服务到系统：
    cp -a ./support-files/mysql.server /etc/init.d/mysql
14. 授权以及添加服务：
    chmod +x /etc/init.d/mysql
    chkconfig --add mysql
15. 启动mysql服务：systemctl start mysql；
16. 查看服务状态：systemctl status mysql；
17. 将mysql命令添加到服务：ln -s /usr/local/mysql/bin/mysql /usr/bin；
18. 用上面初始化完信息生成的临时密码登录：mysql  mysql -uroot -p；
19. 修改root密码：ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';；
20. 执行 flush privileges; 使密码生效；
21. 选择mysql数据库 use mysql;；
22. 修改远程连接并生效,退出：
    update user set host='%' where user='root';
    flush privileges;
    exit;
