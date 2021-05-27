---
title: 一、Mac-安装Flutter-环境-VSCode-配置
date: 2021-05-26 22:12:15
tags: [Flutter, Dart]
---
# 在Mac上搭建Flutter环境
<!--more-->
# 安装Dart
使用终端 输入以下命令
``` sh
 brew tap dart-lang/dart
 brew install dart
```
![](dart_lang_install.png)
![](dart_install.png)
如果电脑没有安装 `brew` 则先使用以下命令安装brew
``` sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
# VSCode
下载[VSCode](https://macwk.com/soft/visual-studio-code)  并根据提示安装
安装Dart插件
![](vscode_install_dart.png)
安装code runner 插件
![](vscode_install_code_runner.png)

# 测试代码
新建一个test.dart文件，输入以下代码
``` dart
main(){
    print("Hello word");
}
```
运行之后会在控制台输出`Hello word`则证明环境配置完成

# 遇到的问题
## brew install dart 失败
报错如下
``` sh
==> Installing dart from dart-lang/dart
==> Downloading https://storage.googleapis.com/dart-archive/channels/stable/release/2.13.1/sdk/dartsdk-macos-x
-=O=-         #    #    #     #
curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to storage.googleapis.com:443
Error: Failed to download resource "dart"
Download failed: https://storage.googleapis.com/dart-archive/channels/stable/release/2.13.1/sdk/dartsdk-macos-x64-release.zip
```
原因嘛，就是下载失败了
解决办法，使用brew缓存，仔细观察报错信息，你会发现下载失败的时候，下载地址已经暴露出来了，你可以手动下载这个包到本地，然后放到brew的缓存目录下
获取brew缓存路径
``` sh
brew --cache
/Users/jackchen/Library/Caches/Homebrew
```
下载并拷贝安装包到此目录之后再次执行 `brew install dart`, 不出意外的话会成功
出了意外请参考以下链接
[mac安装dart报错curl: (35) LibreSSL SSL_connect: SSL_ERROR_SYSCALL in connection to storage.googleapis.com](https://blog.csdn.net/qq_40028324/article/details/105692357)

*再提供一个最终办法，用手机热点*