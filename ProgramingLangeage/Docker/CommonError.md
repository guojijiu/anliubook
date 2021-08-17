###常见错误

问题：
配置nginx和php以后，访问主页，file not found，nginx错误日志：FastCGI sent in stderr: "Primary script unknown" while reading response header from upstream, client: 171.43.83.194
解决：
原因：
1. 先只配置nginx，在设置的nginx访问web的根目录看看能否通；
2. 通了，删除nginx容器，再配置php容器，nginx容器
3. 使用docker exec登陆php容器，配置一个index.php，看看访问的是不是容器配置的地址，如果是，说明nginx.conf配置的有问题
4. 修改nginx配置，重启还是不行，则可能是访问权限的问题（未验证），或者修改php的项目根目录为/var/www/html（可以查看对应目录的权限），重启即可（已验证）
解决：/docker/www:/www 修改为 /docker/www:/var/www/html

问题：
Version in "./docker-compose.yml" is invalid. You might be seeing this error because you're using the wrong Compose file version. Either specify a supported version (e.g "2.2" or "3.3") and place your service definitions under the `services` key, or omit the `version` key and place your service definitions at the root of the file to use version 1.
解决：
将顶部的版本信息改为，version: "2"，docker-compose无法检测安装的compose版本

问题：
mysql5.7    | /usr/local/bin/docker-entrypoint.sh: line 378: exec: mysql5.7: not found
解决:
删除一直重试的命令
command: mysql5.7 -g 'daemon off;'

问题：
docker，rabbitmq配置反响代理报错502
解决：
因为nginx在docker中，所以不能使用127.0.0.1:15672来访问宿主机里的rabbitmq应用，docker内部实际上实现了一个虚拟网桥docker0，所以要通过宿主机内网地址（172.17.0.4）来访问
```
server {
    listen    80;
    server_name rabbitmq.anliu.life;
    index  index.html index.htm;

    location / {
        proxy_pass   http://172.17.0.4:15672;
    }
}
```

问题：
gitlab迁移从a到b报错Permission denied @ rb_sysopen - /opt/gitlab/embedded/service/gitlab-rails/log/application.log
解决：
将opt目录下的gitlab目录迁移过去
