### 部署对象存储服务

### 文档
```
英文文档：https://docs.min.io/?ref=con
中文文档：http://docs.minio.org.cn/docs/master/minio-monitoring-guide
```

### Linux直接安装

```
1. 创建目录：mkdir /usr/local/minio；
2. 下载安装包：wget https://dl.min.io/server/minio/release/linux-amd64/minio；
3. 赋予执行权限：chmod +x minio；
4. 启动：./minio server --config-dir /usr/local/minio/etc /usr/local/minio/data；
```

### docker-composer安装
```
1. 创建目录：mkdir /usr/local/minio；
2. 创建docker-compose.yml，并编写内容，参考docker-compose.yml；
3. 在/usr/local/minio，创建config目录和data目录；
4. 执行：docker-compose up -d；
```

### docker-compose.yml
```
version: '3.2'
services:
    minio:
        image: minio/minio:latest
        container_name: minio
        ports:
            # api端口
            - "17017:9000"
            # 控制台端口
            - "3100:9001"
        volumes:
            - "./data:/data"
            - "./config:/root/.minio"
        environment:
            MINIO_ROOT_USER: "root"
            MINIO_ROOT_PASSWORD: "123456qwe!@#"
        healthcheck:
            test: ["CMD", "curl", "-f", "http://localhost:9000/minio/health/live"]
            interval: 30s
            timeout: 20s
            retries: 3
        #user: deploy
        command: server /data --console-address ":9001" --address ":3100"
        logging:
            driver: "json-file"
            options:
                max-size: "1m"
```