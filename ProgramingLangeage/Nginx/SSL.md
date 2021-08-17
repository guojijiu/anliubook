### 配置SSL

###腾讯云配置ssl
1. 进入腾讯云后台申请免费证书；
2. 等待一分钟后，下载证书；
3. 登录服务器，将证书部署到nginx对应conf下；
4. 重启nginx服务；
示例：

```
server {
    #监听443端口
    listen 443 ssl;
    #你的域名
    server_name aaa.com;

    index  index.html index.htm;

    #ssl on;
    #ssl证书的pem文件路径
    ssl_certificate  /etc/nginx/conf.d/ssl/aaa/1_aaa.crt;
    #ssl证书的key文件路径
    ssl_certificate_key /etc/nginx/conf.d/ssl/aaa/2_aaa.key;
}
server {
    listen 80;
    server_name aaa.com;
    #将请求转成https
        rewrite ^(.*)$ https://$host$1 permanent;
}
```