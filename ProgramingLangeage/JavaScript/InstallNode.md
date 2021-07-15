### 安装nodejs

1. 下载包：http://nodejs.cn/download/；
2. 解压：tar -xvf   node-v6.10.0-linux-x64.tar.xz；
3. 重命名：mv node-v6.10.0-linux-x64  nodejs 
4. 建立软连接：
ln -s /usr/local/nodejs/bin/npm /usr/local/bin/
ln -s /usr/local/nodejs/bin/npm /usr/bin/
ln -s /usr/local/nodejs/bin/node /usr/local/bin/
ln -s /usr/local/nodejs/bin/node /usr/bin/
5. 查看是否安装node -v，且版本是否为最新

6. 更换源yarn config set registry https://registry.npm.taobao.org/

7. 安装yarn global add @vue/cli @vue/cli-service-global
