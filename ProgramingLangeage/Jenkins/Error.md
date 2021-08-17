### 错误

```
构建的时候报错
phpunit 在本机可以运行，在Jenkins的slave上无法执行；
出现/usr/bin/env : php permission denied

解决方法：
设置php路径 
sudo ln -s /application/bin/php/bin/php (your php path) /usr/bin/php
```

```
忽然无法自动构建

解决办法：
查看系统管理->系统配置gitlab配置，是否正常，
如果错误，则登录gitlab，到全局访问令牌中，添加apitoken
```