### 配置邮箱

### 企业邮箱
 gitlab_rails['smtp_enable'] = true
 gitlab_rails['smtp_address'] = "smtp.exmail.qq.com"
 gitlab_rails['smtp_port'] = 465
 gitlab_rails['smtp_user_name'] = "123@metware.cn"
 gitlab_rails['smtp_password'] = "123"
 gitlab_rails['smtp_domain'] = "exmail.qq.com"
 gitlab_rails['smtp_authentication'] = "login"
 gitlab_rails['smtp_enable_starttls_auto'] = true
 gitlab_rails['smtp_tls'] = true
 gitlab_rails['gitlab_email_from'] = '123@metware.cn'

### 配置完之后，执行编译 重启 测试
gitlab-ctl reconfigure
gitlab-ctl restart
