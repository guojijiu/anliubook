### 错误总结

错误：页面显示：has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
排查：nginx日志报错upstream timed out (60: Operation timed out) while reading response header from upstream
解决方案：
修改nginx.conf，增加proxy_read_timeout 300;
修改对应的server下的代理php里面，增加：
fastcgi_connect_timeout 900; 
fastcgi_read_timeout 900; 
fastcgi_send_timeout 900;
修改php.ini，修改max_execution_time = 300 ;