---
title: 一、Mac-安装GO-环境-VSCode-配置
date: 2021-06-22 17:00:34
tags: [GO]
---
# 前言
从本文开始，记录自己学习GO语言的一切
<!--more-->
# 开发包安装
``` sh
# 官网
http://golang.org/
# 国内镜像
https://golang.google.cn/
```
![](go_down_1.png)
![](go_down_2.png)
# 验证
``` sh
go version
# 输出以下信息证明成功
```
![](go_version.png)
# 安装配置VSCode 
[官网](https://code.visualstudio.com/)
下载安装即可
## 安装中文插件
![](vscode_chinese.png)
## 安装GO开发包
这一步直接点击install all，但是大概率会失败，需要修改GOPROXY
![](VSCode_go.png)
# 修改GOPROXY
``` sh
# 输入
go env
# 检查GOPROXY
GOPROXY="https://proxy.golang.org,direct"
```
需要将GOPROXY修改为`https://goproxy.cn,direct`
``` sh
# 修改代理
go env -w GOPROXY=https://goproxy.cn,direct
# 安装需要的所有插件
go get -v golang.org/x/tools/gopls
# 或者在VSCode中点击install all
```
# 尝试第一个go程序
![](go_new_file.png)
``` go
package main

import "fmt"

func main() {
	fmt.Println("Hello word")
}
```
![](go_run_file.png)
![](go_run_file_result.png)
每次运行之前需要先执行保存操作
至此GO环境与VSCode配置完成。