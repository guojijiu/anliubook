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
 添加国内源：yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
 添加官方源：yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

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

1. 访问https://github.com/docker/compose/releases/latest，获得最新的docker-compose版本

2. 下载最新版本的 docker-compose 到 /usr/bin 目录下
```
curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` -o /usr/bin/docker-compose
```

3. 给 docker-compose 授权
```
chmod +x /usr/bin/docker-compose
```