### 常见问题

### 部署swoole项目，如何解决跨域问题 参考：（https://www.freesion.com/article/86401061237/）
location @swoole {
        set $suffix "";

        if ($uri = /index.php) {
            set $suffix ?$query_string;
        }

        proxy_http_version 1.1;
        proxy_set_header Host $http_host;
        proxy_set_header Scheme $scheme;
        proxy_set_header SERVER_PORT $server_port;
        proxy_set_header REMOTE_ADDR $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        add_header "Access-Control-Allow-Origin" "*"; # 设置允许跨域
        add_header "Access-Control-Allow-Methods" "POST,GET,PUT,OPTIONS,DELETE"; # 设置允许通过跨域方法
        add_header "Access-Control-Allow-Credentials" "true"; # 设置是否允许发送 cookies
        add_header "Access-Control-Expose-Headers" "Content-Disposition,Accept-Length";
        add_header "Access-Control-Allow-Headers" "Content-Type,Access-Control-Allow-Origin,Access-token,Content-Length,Accept-Encoding,X-Requested-with, Origin,Access-Control-Allow-Methods,token"; #设置允许跨域的header
        # IF https
        # proxy_set_header HTTPS "on";

        proxy_pass http://127.0.0.1:1215$suffix;
}

