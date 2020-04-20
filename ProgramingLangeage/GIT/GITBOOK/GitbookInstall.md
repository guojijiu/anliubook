#介绍
Gitbook 是基于Node.js的命令行工具，用来创建漂亮的电子书，它使用Markdown或AsciiDoc语法来撰写内容，用Git进行版本控制，且可以托管在Github上。Gitbook 可以将作品编译成网站、PDF、ePub 和 MOBI 等多重格式

#安装
1. 安装node.js，在[node.js官网](https://nodejs.org/en/)下载，直接安装稳定版本；
2. 检测 node.js 是否安装成功，命令***npm -v***；
3. 安装 gitboot 和命令行工具 -g 代表全局安装
```
sudo npm install gitbook -g 
sudo npm install -g gitbook-cli
```
4. 检测是否安装成功 v 大写
```
gitbook -V
gitbook -version
```
5. 更新 gitbook 命令行工具
```
sudo npm update gitbook-cli -g
```
6. 卸载 GitBook 命令
```
sudo npm uninstall gitbook-cli -g
```
7. 查看安装位置
```
which gitbook
```
8. 安装 [gitboot editor](https://legacy.gitbook.com/editor/osx),方便编辑书籍
9. 将安装的calibre放在应用程序中,执行
```
sudo ln -s /Applications/calibre.app/Contents/MacOS/ebook-convert /usr/local/bin
```
10. 创建 mygitbook 文件夹，作为第一本书,并切换到这个文件夹下面
```
mkdir mygitbook && cd mygitbook
```
11. 目录建好以后在根目录下执行命令,只支持2级目录：
```
gitbook init
```
12. 编写 gitbook 内容,重新编译
```
gitbook build
```
13. 在根目录执行命令,启动服务：
```
gitbook serve
```
14. 访问,用浏览器打开 http://localhost:4000/ 或 http://127.0.0.1:4000/ 查看显示书籍的效果。结束预览 ctrl+c
15. 生成电子书,依赖于Calibre
```
gitbook mobi ./ ./MyFirstBook.mobi
```

#关联github
1. 在github上创建对应的项目，然后将本地的git公钥上传到github上，具体如何创建，如果不清楚，百度即可；
2. 创建完以后，在项目根目录执行如下命令
```
git init
git add .
git commit -m "first commit"
git remote add orign git@github.com:guojijiu/anliubook.git
git push -u orign master
```
#关联gitbook
1. 登陆[gitbook](https://www.gitbook.com/)，使用github账号登陆就行；
2. 创建一个gitbook；
3. 进入刚创建的gitbook，进入settings/integrations，
4. 选择github，将在github上创建的gitbook项目进行关联即可。