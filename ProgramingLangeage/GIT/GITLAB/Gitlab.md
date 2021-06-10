### 备份与恢复
1. 开启config/gitlab.rb的配置
gitlab_rails['backup_path'] = "/var/opt/gitlab/backups"  # 备份的目录
gitlab_rails['backup_archive_permissions'] = 0644  # 备份包（tar格式压缩包）的权限
gitlab_rails['backup_keep_time'] = 604800  # 备份的保留时间，单位是秒
2. gitlab-ctl reconfigure  # 重载配置，使之生效
3. gitlab-rake gitlab:backup:create  # 执行备份
4. 还原备份
    注意：备份目录和gitlab.rb中定义的备份目录必须一致
         GitLab的版本和备份文件中的版本必须一致，否则还原时会报错。
gitlab-rake gitlab:backup:restore BACKUP=1621847259_2021_05_24_13.10.0  # 还原
5. gitlab-ctl restart  # 重启服务
6. gitlab-rake gitlab:check SANITZE=true  # 检查GitLab所有组件是否运行正常


### 每次更新代码需要输入密码
1.执行git config --global credential.helper store
2.git pull代码，再次输入一次账号密码