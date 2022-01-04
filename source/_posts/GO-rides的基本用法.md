---
title: GO-rides的基本用法
date: 2021-07-01 16:24:56
tags: [GO, rides]
---
**Rides的基本用法，以及go如何使用Rides**


<!-- more -->
# 安装Redis
```sh
brew install redis
```
# 启动Redis

```sh
redis-server /usr/local/etc/redis.conf
```
# 连接Redis
```sh
redis-cli
127.0.0.1:6379>
```
如果6379被占用，可以修改`/usr/local/etc/redis.conf`中的port
例如：port=8081
*--raw 可以输出中文*
```sh
redis-cli -p 8081 --raw
```

# Redis基本命令
## 存
```sh
127.0.0.1:8081> set name Jack
OK
```
## 取
```sh
127.0.0.1:8081> get name
Jack
```
## 删
DEL key
```sh
127.0.0.1:8081> del name
1
127.0.0.1:8081> get name

```
## 序列化
DUMP key
```sh
127.0.0.1:8081> set content "Hello word!"
OK
127.0.0.1:8081> dump content
"\x00\x0bHello word!\t\x00\x19A\x81\x97\xfa}\xb9\x86"
```
## 检查给定 key 是否存在
EXISTS key
1: 存在
0: 不存在
```sh
127.0.0.1:8081> EXISTS name
(integer) 1
127.0.0.1:8081> EXISTS age
(integer) 0
```
## 为给定 key 设置过期时间
### 以秒计
EXPIRE key seconds
```sh
127.0.0.1:8081> EXPIRE name 10
(integer) 1
# 十秒之后
127.0.0.1:8081> get name
(nil)
```
### 毫秒
PEXPIRE key milliseconds
```sh
```
### 时间戳-秒
EXPIREAT key timestamp
```sh
```
### 时间戳-毫秒
PEXPIRE key milliseconds
```sh
```
## 找到所有复合规则的key
KEYS pattern
```sh
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> set name1 Jack1
OK
127.0.0.1:8081> keys n*
1) "name"
2) "name1"
```
## 移除 key 的过期时间，key 将持久保持
PERSIST key
```sh
127.0.0.1:8081> EXPIRE name 100
(integer) 1
127.0.0.1:8081> PERSIST name
(integer) 1
```
## 显示key距离过期剩余时间-毫秒
PTTL key
```sh
127.0.0.1:8081> EXPIRE name 100
(integer) 1
127.0.0.1:8081> PTTL name
(integer) 93169
# 已过期的返回 -2
127.0.0.1:8081> PTTL name
(integer) -2
```
## 显示key距离过期剩余时间-秒
TTL key
```sh
127.0.0.1:8081> EXPIRE name 100
(integer) 1
127.0.0.1:8081> TTL name
(integer) 92
# 已过期的返回 -2
127.0.0.1:8081> TTL name
(integer) -2
```
## 随机返回一个key
RANDOMKEY
```sh
127.0.0.1:8081> RANDOMKEY
"sex"
127.0.0.1:8081> RANDOMKEY
"name1"
127.0.0.1:8081> RANDOMKEY
"content"
127.0.0.1:8081> RANDOMKEY
"sex"
127.0.0.1:8081> RANDOMKEY
"content"
127.0.0.1:8081> RANDOMKEY
"sex"
```
## 修改Key
RENAME key newkey
*替换newkey的值 key置空*
```sh
127.0.0.1:8081> RENAME name myname
OK
127.0.0.1:8081> get myname
"Jack"
127.0.0.1:8081> get name
(nil)
```
RENAMENX key newkey
*当newkey存在时，重命名失败*
```sh
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> RENAME name myname
OK
127.0.0.1:8081> get myname
"Jack"
127.0.0.1:8081> get name
(nil)
127.0.0.1:8081> set name chen
OK
127.0.0.1:8081> RENAMENX name myname
(integer) 0
127.0.0.1:8081> RENAME name myname
OK
127.0.0.1:8081> get name
(nil)
127.0.0.1:8081> get myname
"chen"
```
## 返回 key 所储存的值的类型
TYPE key
*redis 中没有int值，会使用string存入*
```sh
127.0.0.1:8081> set age 12
OK
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> TYPE age
string
127.0.0.1:8081> TYPE name
string
```
# Redis String 基本命令
## 存取
```sh
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> get name
"Jack"
```
## 返回 key 中字符串值的子字符
GETRANGE key start end
*类似于字符串截取, 从哪个下标开始截取到哪里，包含end*
```sh
# 截取0,1,2
127.0.0.1:8081> GETRANGE name 0 2
"Jac"
```
## 将给定 key 的值设为 value ，并返回 key 的旧值
```sh
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> GETSET name chen
"Jack"
127.0.0.1:8081> get name
"chen"
```
## 对 key 所储存的字符串值，获取指定偏移量上的位(bit)
*当偏移量 OFFSET 比字符串值的长度大，或者 key 不存在时，返回 0*
```sh
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> GETBIT name 1
(integer) 1
127.0.0.1:8081> GETBIT name 4
(integer) 0
```
## 获取所有(一个或多个)给定 key 的值
```sh
127.0.0.1:8081> set age 12
OK
127.0.0.1:8081> set name Jack
OK
127.0.0.1:8081> MGET name age
1) "chen"
2) "12"
```
## 将值 value 关联到 key ，并将 key 的过期时间设为 seconds 
以秒为单位
SETEX key seconds value
以毫秒为单位
PSETEX key milliseconds value
```sh
127.0.0.1:8081> SETEX name 30 jack
OK
127.0.0.1:8081> ttl name
(integer) 12
127.0.0.1:8081> get name
"jack"
```
## 只有在 key 不存在时设置 key 的值
SETNX key value
```sh
127.0.0.1:8081> SETNX name Jack
(integer) 1
127.0.0.1:8081> SETNX name Jack
(integer) 0
```
## 覆盖偏移量之后的字符
SETRANGE key offset value
```sh
127.0.0.1:8081> SET content "Hello Word"
OK
127.0.0.1:8081> SETRANGE content 6 "Redis"
(integer) 11
127.0.0.1:8081> GET content
"Hello Redis"
```
## 返回 key 所储存的字符串值的长度
STRLEN key
```sh
127.0.0.1:8081> GET content
"Hello Redis"
127.0.0.1:8081> STRLEN content
(integer) 11
```
## 同时设置一个或多个 key-value 对
*覆盖已有key 的value*
MSET key value [key value ...]
*当且仅当所有给定 key 都不存在*
MSETNX key value [key value ...]
```sh
127.0.0.1:8081> MSET name Jack age 12 sex male
OK
127.0.0.1:8081> get name
"Jack"
127.0.0.1:8081> get age
"12"
127.0.0.1:8081> get sex
"male"
```

## 在对应的string值后面追加字符
APPEND key value
```sh
127.0.0.1:8081> get content
"Hello Redis"
127.0.0.1:8081> APPEND content " oh Year!"
(integer) 20
127.0.0.1:8081> get content
"Hello Redis oh Year!"
```
# Redis String 基本命令