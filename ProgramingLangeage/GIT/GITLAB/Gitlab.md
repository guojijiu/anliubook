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

非常注意：迁移完成以后，需要将配置文件手动迁移到新服务器，否则设置等相关功能无法使用。

### 每次更新代码需要输入密码
1.执行git config --global credential.helper store
2.git pull代码，再次输入一次账号密码

### gitlab迁移导致部分页面操作500
解决方案：
-- Clear project tokens
UPDATE projects SET runners_token = null, runners_token_encrypted = null;
-- Clear group tokens
UPDATE namespaces SET runners_token = null, runners_token_encrypted = null;
-- Clear instance tokens
UPDATE application_settings SET runners_registration_token_encrypted = null;
UPDATE application_settings SET encrypted_ci_jwt_signing_key = null;
-- Clear runner tokens
UPDATE ci_runners SET token = null, token_encrypted = null;


### – PG::DataCorrupted: ERROR: could not read block 3 in file "base/16386/48135": read only 0 of 8192 bytes
解决方案：(参考https://blog.csdn.net/pcn01/article/details/107187054/)
1. 登录gitlab容器
2. 查看配置信息cat /var/opt/gitlab/gitlab-rails/etc/database.yml
3. 登录数据库su - gitlab-psql
4. 连接数据库psql -h /var/opt/gitlab/postgresql -d gitlabhq_production
5. 查看损坏的id：select relname,relkind from pg_class where relfilenode=2702（根据报错选择）;
6. 修复：set zero_damaged_pages = on;
7. 修复：reindex index pg_trigger_oid_index;
8. 重新fork项即可。