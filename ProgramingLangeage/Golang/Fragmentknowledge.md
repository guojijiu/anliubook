### 碎片知识

1. golang包管理分类

```
go path：最早的依赖包管理方式（Go1.0），启用GOPATH模式，要求将所有工程代码要求放在GOPATH/src目录下；
govendor：在 Go 1.5版本之后，Go 提供了 GO15VENDOREXPERIMENT 环境变量(Go 1.6版本默认开启该环境变量)和 Govendor 包管理工具，用于将 go build 时的应用路径搜索调整成为当前工程/vendor 目录的方式，有效的解决了不同工程使用自己独立的依赖包目录。编译 Go 代码会优先从vendor目录先寻找依赖包，vendor目录如果没有找到，然后在 GOPATH 中查找，都没找到最后在 GOROOT 中查找；
go module：从Go1.11开始，官方推出Go module作为包管理工具，且从Go1.13开始为默认选择启用。GOMODULE模式下所有依赖的包存放在GOPATH/pkg/mod目录下，所有第三方二进制可执行文件放在GOPATH/bin目录下，且工程项目可以放在GOPATH路径之外，但要求项目中需要有go.mod文件（该文件通过go mod init命令初始化可以得到）。
```

2. 常量概念

```
GOROOT：go的安装目录，类似java的jdk，存放一些内置的开发包和工具。
GOPATH：go指定的工作空间，用于保存go项目的代码和第三方依赖包。
```
