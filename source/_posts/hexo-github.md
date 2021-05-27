---
toc: true
title: 使用hexo+github搭建博客
date: 2021-03-13 22:41:51
tags: [干货]
---
*本文将教你如何免费搭建属于自己的博客。如你所看到的。*

# 前言
使用GitHub pages 服务搭建博客的好处

```
1. 全是静态文件，访问速度快；
2. 免费，不花钱搭建自己的博客；
3. 可以绑定自己的域名；
4. 数据安全，可使用Git版本管理；
5. 跨平台编写
```
<!--more-->
## 准备工作
* 一个GitHub账号，最好新注册一个
* Node.js
* git环境（一般自带）
* npm

## 环境配置
### GitHub
创建仓库
![](github_new.png)
![](github_creat_project.png)
配置ssh key
检查是否配置ssh key
``` sh
cat ~/.ssh/id_rsa.pub
``` 
生成ssh key
``` sh
ssh-keygen -t rsa -C "你的Email"
然后一路回车
```
查看ssh key
``` sh
cat ~/.ssh/id_rsa.pub
```
![](cat_sshkey.png)
复制ssh key，打开GitHub setting
![](github_setting.png)
![](github_new_ssh.png)
![](github_creat_sshkey.png)
![](github_sshkey_created.png)
测试ssh key是否可用
``` sh
ssh -T git@github.com # 注意邮箱地址不用改
```
如果提示 *Are you sure you want to continue connecting (yes/no)?* ，输入yes，然后会看到：
`Hi liuxianan! You've successfully authenticated, but GitHub does not provide shell access.`
看到这个信息说明SSH已配置成功！
此时你还需要配置：
``` sh
 git config --global user.name "liuxianan"// 你的github用户名，非昵称
 git config --global user.email  "xxx@qq.com"// 填写你的github注册邮箱
```
至此 ssh key 配置完成
### Node.js
打开[官网](https://nodejs.org/zh-cn/)，下载LTS
![](Node.js_download.png)
![](Node.js_install.png)
双击安装
### Git
检查是否安装Git
``` sh
git --version
```
![](git_version.png)
出现 *git version 2.24.3 (Apple Git-128)* 则证明安装成功
如果没有安装Git 使用以下命令安装
``` sh
brew install git
```
### npm
检查是否安装npm
``` sh
npm --version
```
![](npm_versaion.png)
出现 版本号 则证明安装成功
如果没有安装npm 使用以下命令安装
``` sh
brew install npm
```

*至此环境已经配置完成*

## Hexo配置
### 安装Hexo
使用npm安装hexo
``` sh
sudo npm install hexo -g
```
检查hexo是否安装成功
``` sh
hexo -v
```
出现 *hexo-cli: 4.2.0* 证明安装成功
创建hexo目录（*目录需要为空文件夹*）
``` sh
cd hexo
```
初始化hexo 过程可能会长一些
``` sh
hexo init
```
出现 *INFO  Start blogging with Hexo!* 说明初始化成功
安装所需要的组件
``` sh
npm install
```
![](hexo_npm_install.png)

编译 hexo
``` sh
hexo g
```
![](hexo_g_success.png)
开启服务器 本地体验hexo
``` sh
hexo s
```
![](hexo_s_success.png)
浏览器访问 http://localhost:4000/ 可以看到效果

## 开始写文章
创建第一篇文章
如果你使用 *hexo s* 开启了本地浏览器 需要使用 Ctrl+C
进入hexo文件夹
``` sh
cd hexo
hexo new "My First Blog"
```
![](hexo_creat_blog.png)
![](hexo_creat_blog_finder.png)
开启本地浏览器则可以看到你的文章了
![](hexo_craet_blog_web.png)
如果你需要写文章 只需要编辑 *My-First-Blog.md*即可

## 部署到GitHub上
如果你使用 *hexo s* 开启了本地浏览器 需要使用 Ctrl+C
1、 clean
``` sh
hexo clean
```
![](hexo_clean.png)
2、 编译
```  sh
hexo g
```
![](hexo_g.png)
3、 发布
``` sh
hexo d
```
![](git_d_success.png)
打开 你的github pages 主页
例如 我的是 https://jackchenblog.github.io/ 就能看到你的文章了
![](github.io.png)

## 安装插件
### 图片插件
``` sh
npm install https://github.com/CodeFalling/hexo-asset-image -- save
```
修改 hexo 目录下, _config.yml文件配置
``` sh
post_asset_folder: true
```
此时使用 `hexo n blog-name`创建新文章时会同时创建一个对应名字的文件夹
只需吧你需要的图片放入文件夹，在你需要的位置写入图片语法即可，例如
![](hexo_inster_image.png)

## 绑定自己的域名(看自己需要)
### 购买域名
推荐使用阿里云，首年有优惠
购买完成之后需要实名认证，域名备案，这些可以根据提示完成
### 绑定到你的github.io
打开阿里云控制台，点击域名，你可以看到你购买的域名，点击*解析*
![](domain_name_list.png)
![](domain_name_add.png)
![](domain_name_add_info.png)
> 域名配置最常见有2种方式，CNAME和A记录，CNAME填写域名，A记录填写IP，由于不带www方式只能采用A记录，所以必须先ping一下你的用户名.github.io的IP，然后到你的域名DNS设置页，将A记录指向你ping出来的IP，将CNAME指向你的用户名.github.io，这样可以保证无论是否添加www都可以访问

然后到你hexo->public目录新建一个名为CNAME的文件（无后缀），里面填写你的域名
可直接使用命令生成 并发布到GitHub
``` sh
cd hexo
echo abcd.com > ./public/CNAME
hexo d -g
```
![](CNAME_finder.png)

稍等几分钟，访问你购买的域名，你就能看到你的博客了。

## 错误处理
### hexo s 失败(端口被占用)
``` sh
hexo s
```
![](hexo_s_port_used.png)
hexo 默认端口为4000 首先检查4000端口是谁在占用
``` sh
lsof -i tcp:4000
```
![](port_check.png)
占用4000端口的为node 如果你认识这个进程 可以使用以下命令结束它
如果你不认识它 你也可以kill掉他 不过 不保证有什么后果😛
``` sh
sudo kill -9 3531
```
3531 为PID 所对应的值  结束进程成功有如下提示
``` sh
[1]  + 3531 killed     xxx
```

### hexo tags 404
#### 解决办法
在hexo目录中执行一下命令
``` sh
hexo new page "tags" 
hexo new page "categories"
```
打开它们并相应添加type: "tags"和type: "categories"，保存
![](hexo_add_tags.png)
![](hexo_add_categories.png)
