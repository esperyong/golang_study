# gomodule基础

## 1.快速开始
gomodule是从go1.11版本后出来的，需要先开启gomodule。
[相关的博文](https://maelvls.dev/go111module-everywhere/)

### 开启GO111MODULE
```shell
$ go env -w GO111MODULE=on
$ go env (打印go相关的环境变量)
```
### 安装一个用Go开发的第三方软件
```shell
go install github.com/go-kratos/kratos/cmd/kratos/v2@latest
```
安装到GOPATH中的相应目录中。

之后要将GOPATH/bin目录加载到环境变量PATH中，我本地使用的的homebrew安装的golang，
我的bin路径在：/Users/liuyong/go/bin。
因此要在启动脚本中增加：
```shell
export PATH="/Users/liuyong/go/bin:$PATH”
```


模块是Go packages的集合，以 go.mod 文件的形式存储在文件树的根目录。
go.mod 定义了模块的 模块路径 （根目录的导入路径）和 依赖项需求 （成功构建所需要的其他模块）。
每一个依赖项需求都以模块路径和特定**语义版本**的形式给出。

从 Go 1.11 开始，当前目录或者任何父目录包含 go.mod 文件时，Go 命令就可以使用模块，前提是该目录要在 $GOPATH/src 之外。
（在 $GOPATH/src 内的目录，出于兼容性考虑，Go 命令仍然以旧的 GOPATH 模式运行，即使存在 go.mod 文件。详细内容请参阅 Go 命令文档 ）。

**从 Go 1.13 开始，模块模式将是所有开发的默认模式。**

本文介绍了使用GoModule模块进行Go开发的一系列常见操作：
- ### 创建新模块
```shell
go mod init example.com/hello
```
- ### 添加依赖
```shell
go get golang.org/x/text
```
- ### 升级依赖
- ### 添加对新的主版本的依赖。
```shell
go get rsc.io/sampler
```
- ### 将依赖升级到新的主版本。
```shell
go get rsc.io/sampler
```
- ### 删除未使用的依赖。
```shell
go mod tidy
```

- ### 用go list 查看当前模块的所有依赖：
命令 
```shell
go list -m all
```
会列出当前模块的所有依赖
加上json选项会输出更详尽的内容
```shell
$ go list -m -json all
```
查看某一个第三方模块的所有版本
```shell
go list -m -versions rsc.io/sampler
```

- ### 用go get 来下载依赖的第三方包：
默认为 **@latest**，对应「最新」版本。
```shell
$ go get golang.org/x/text
```
**下载第三方包的某一个版本：**
```shell
go get rsc.io/sampler@v1.3.1
```
- ### 用 go mod tidy 命令来清除这些未使用的依赖
```shell
$ go mod tidy 
```
会检查你需要依赖的包，如果你代码中使用到的第三方包没有在 go.mod 文件中，它将会把此包放入到你的 go.mod 文件中，并且它也会移除 go.mod 中你没有使用过的第三方包。
如果你导入的第三方包还没有迁移为 go module 项目，那么将会在此包后面加入 // indirect 的备注，
**因此，提交 go.mod 文件到版本控制中心前最好先运行一下 ```go mod tidy```，这是一个比较好的习惯。**

对于没有go.mod的项目，可以先go mod init之后，再执行go mod tidy。

如果你发现某个包的版本不是你想要的，
你可以通过这两个命令```go mod why -m```和```go mod graph```来找出原因，
并且通过```go get```升级或降级为你想要的版本.
```shell
$ go mod why -m rsc.io/binaryregexp
```
```shell
$ go mod graph | grep rsc.io/binaryregexp
```
```shell
$ go get rsc.io/binaryregexp@v0.2.0
```
- ### 使用 Go 模块等相关命令的工作流：
1. ```go mod init```创建一个新模块，会初始化一个go.mod并有其描述.
1. ```go build```, ```go test```, 以及其他包生成命令根据需要向go.mod添加新的依赖项.
1. ```go list -m all```打印当前模块的依赖.
1. ```go get```更改依赖项的所需版本 (或添加一个新的依赖项).
1. ```go mod tidy```移除未使用的依赖
