### 安装

1. 参考地址[http://www.yihaomen.com/article/1827.html]

2. 修改角色 /etc/sysconfig/jenkins ，JENKINS_USER=deploy

3. 修改对应的权限： 
chown -R deploy:deploy /var/lib/jenkins
chown -R deploy:deploy /var/cache/jenkins
chown -R deploy:deploy /var/log/jenkins

5. 重启服务
systemctl restart jenkins

6. 进入管理界面；
http://127.0.0.1:8280/

7. 安装插件：
Display Console Output （由于jenkins不会实时将build脚本的输出给拿出来，这个相当必要(jenkins脚本查看build过程,调试)

3. 构建命令
 1. jenkins容器内安装ssh-keygen；
 2. 将公钥添加到gitlab账号上中；
 3. 在源码管理->git中添加git地址，如：http://172.16.0.106:8929/gitlab/metware/cloud_platform/cloud_platform.git；
 4. 添加Credentials用户，公钥添加的账户上；