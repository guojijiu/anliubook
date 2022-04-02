### 安装docker-ce
### 以下操作均为root
### 参考地址：https://www.jianshu.com/p/7d9ff93bc89e
### 官方地址：https://docs.docker.com/engine/install/centos/
1. 升级内核
```
yum -y update
```

2. 如果不升级内核
```
yum -y upgrade
```

3. 安装必备依赖
```
yum install -y yum-utils device-mapper-persistent-data lvm2
```

4. 添加yum的源
```
 添加国内源：yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 或者
 添加官方源：yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

5. 安装docker-ce
```
最新版：
yum -y install docker-ce
或者指定版本：
查看版本：yum list docker-ce --showduplicates|sort -r
指定版本安装：yum install docker-ce-19.03.12
```

6. 启动服务
```
systemctl start  docker
```

7. 查看版本
```
docker -v
```

8. 设置为开机自动激活单元并现在立刻启动
```
systemctl enable --now docker
```

### 安装Docker-Compose

1. 获得最新的docker-compose版本：https://github.com/docker/compose/releases/latest，

2. 下载最新版本的 docker-compose，版本号1.23.2替换为想要的版本， 到 /usr/bin 目录下
```
curl -L https://github.com/docker/compose/releases/download/1.29.2/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
```

3. 给 docker-compose 授权
```
chmod +x /usr/bin/docker-compose
```

### composer autoload优化机制
```
第一层级(Level-1)优化：生成 classmap
如何运行：
执行命令 composer dump-autoload -o （-o 等同于 --optimize）

原理：
这个命令的本质是将 PSR-4/PSR-0 的规则转化为了 classmap 的规则， 因为 classmap 中包含了所有类名与类文件路径的对应关系，所以加载器不再需要到文件系统中查找文件了。可以从 classmap 中直接找到类文件的路径。

注意事项
建议开启 opcache , 这样会极大的加速类的加载。
php5.5 以后的版本中默认自带了 opcache 。
这个命令并没有考虑到当在 classmap 中找不到目标类时的情况，当加载器找不到目标类时，仍旧会根据PSR-4/PSR-0 的规则去文件系统中查找

第二层级(Level-2/A)优化：权威的（Authoritative）classmap
执行命令：
执行命令 composer dump-autoload -a （-a 等同于 --classmap-authoritative）

原理
执行这个命令隐含的也执行了 Level-1 的命令， 即同样也是生成了 classmap，区别在于当加载器在 classmap 中找不到目标类时，不会再去文件系统中查找（即隐含的认为 classmap 中就是所有合法的类，不会有其他的类了，除非法调用） 

注意事项
如果你的项目在运行时会生成类，使用这个优化策略会找不到这些新生成的类。

第二层级(Level-2/B)优化：使用 APCu cache
执行命令：
执行命令 composer dump-autoload --apcu

原理：
使用这个策略需要安装 apcu 扩展。

apcu 可以理解为一块内存，并且可以在多进程中共享。

这种策略是为了在 Level-1 中 classmap 中找不到目标类时，将在文件系统中找到的结果存储到共享内存中， 当下次再查找时就可以从内存中直接返回，不用再去文件系统中再次查找。

在生产环境下，这个策略一般也会与 Level-1 一起使用， 执行composer dump-autoload -o --apcu, 这样，即使生产环境下生成了新的类，只需要文件系统中查找一次即可被缓存 ， 弥补了Level-2/A 的缺陷。

如何选择优化策略？
要根据自己项目的实际情况来选择策略，如果你的项目在运行时不会生成类文件并且需要 composer 的 autoload 去加载，那么使用 Level-2/A 即可，否则使用 Level-1 及 Level-2/B是比较好的选择。

原文：https://blog.csdn.net/raoxiaoya/article/details/107833195
```
