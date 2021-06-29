---
title: GO-gin框架的使用
date: 2021-06-29 17:53:38
tags: [GO, gin]
---
**GO gin框架的使用以及遇到的坑**

<!--more-->

# 安装gin
```sh
go get -u github.com/gin-gonic/gin
```
## 坑！！安装完成之后在项目中无法使用gin框架
报错如下
![](gin_err.png)

解决办法，进入到对应的文件夹，使用`go mod`添加引用库。
例如我的文件夹名字是server

```sh
cd server
go mod init server
go mod tidy # 添加引用
```