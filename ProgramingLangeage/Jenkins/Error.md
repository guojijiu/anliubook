### 错误

```
构建的时候报错
phpunit 在本机可以运行，在Jenkins的slave上无法执行；
出现/usr/bin/env : php permission denied

解决方法：
设置php路径 
sudo ln -s /application/bin/php/bin/php (your php path) /usr/bin/php
```
