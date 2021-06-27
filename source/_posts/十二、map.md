---
title: 十二、map
date: 2021-06-26 22:45:51
tags: [GO]
---

**map的基本概念及用法**

<!--more-->

# 概念
- Go语言中 map（又叫映射）是一种特殊的数据类型：一种键(key)值(value)对组成的无序集合.类似于swift字典。
- 声明格式 var mapName map[keyType]valueType


# 示例

```go
myMap := make(map[string]int, 5)
// 获取长度
len(myMap)
// 获取容量
cap(myMap)
// 赋值
myMap["jack"] = 666
// 修改
myMap["jack"] = 888
// 删除
delete(myMap, "jack")      // 键不存在时，不操作，不报错
// 遍历
for k, v := range myMap{
    fmt.Println(k, v)
}
```
# map做函数的参数

- map是引用类型，做函数参数修改形参将影响实参
