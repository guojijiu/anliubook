### 初始化项目

1. 开启go module
```
GO111MODULE=on
```
2. 配置代理
```
GOPROXY="https://goproxy.io,direct"
```
3. 创建项目
4. 初始化mod
```
go mod init project
```
5. 检测并更新依赖
```
go mod tidy
```
6. 下载依赖
```
go mod download
```
7. 配置Goland IDE
```
Goland->Preferences->Go->Go Modules->勾选Enable Go modules integration
```
