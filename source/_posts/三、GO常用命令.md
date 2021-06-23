---
title: 三、GO常用命令
date: 2021-06-23 16:20:36
tags: [GO]
---
~
<!--more-->
# 常用命令
```go
build:编译包和依赖
clean:移除对象文件(go clean :删除编译的可执行文件)
doc:显示包或者符号的文档
env:打印go的环境信息
bug:启动错误报告
fix:运行go tool fix 
fmt:运行gofmt进行格式化(go fmt : 自动将代码格式)
generate:从processing sour ce生成go文件
get:下载并安装包和依赖 (go get github.com/astaxie/beego:下载beego框架)
insta11:编译并安装包和依赖(go instal1 项目名:会将go编译，并放到bi n路径下)
list:列出包
run:编译并运行gg程序
test:运行测试
tool:运行go提供的工具
version:显示go的版本
vet:运行go too1 vet
```
## go build
* 将.go文件编译成可执行文件
* 在项目目录下执行go build
* 在其他路径下执行go build,需要在后面加上路径

## go run
* 一步到位，直接运行

## go install
* 先执行go build,然后拷贝可执行文件到gopath/bin/.因为gopath/bin在环境变量中， 所以可直接运行该文件。该命令相当于安装软件。

## go get
* 这个命令同样也是很常用的，我们可以使用它来下载并安装第三方包。
