---
title: 三、Dart 基本数据类型
date: 2021-05-28 21:47:49
tags: [Flutter, Dart]
---
# 前言
Dart中基本数据类型的介绍以及使用，包括String, int, double, list, bool, maps

<!--more-->

# String
## 定义
``` dart
var str1 = 'This is str1';
var str2 = "This is str2";
String str3 = 'This is str3';
String str4 = "This is str4";
String str5 = '''This is str5,
This is str5 too''';
String str6 = """This is str6,
This is str6 too""";
/* 
区别
一对单引号和一对双引号定义的String基本无区别
三对单引号和三对双引号中定义的String可以换行。
*/

//使用r创建原始raw字符串（转义字符等特殊字符会输出出来，而不会自动被转义）
String str7 = r'This is \n str7';
print(str3);
// This is str3
print(str7);
// This is \n str7;
```

## 常用属性
``` dart
String str1 = "This is str1";
String str2 = "";
/// 长度
print(str1.length);
// 12
/// 是否为空
print(str1.isEmpty);
// false
print(str2.isEmpty);
// true
/// 是否不为空
print(str1.isNotEmpty);
// true
print(str2.isNotEmpty);
// false
```

## 字符串拼接
``` dart
String str1 = 'Hello';
String str2 = 'Word';
print("$str1 $str2");
// Hello Word
print(str1+str2);
// HelloWord
```

## 字符串切割
``` dart

```

## 字符串模板，使用${} 将一个字符串变量/表达式嵌入到另一个字符串内
在字符串中嵌入变量
``` dart
String str1 = "aa";
String str2 = "bb${str1}bb";
print(str2); 
// bbaabb
```
在字符串中嵌入函数
``` dart
String str1 = "aa";
String str2 = "bb${str1.toUpperCase()}bb";
print(str2); 
// bbAAbb
```

# 数据类型之间的转换
## 字符串与数字之间的转换
### 字符串转int数值
``` dart
int age = int.parse("12");
print(age); 
// 33
```
### 字符串转double数值
``` dart
double price = double.parse("5.28");
print(price); 
// 5.28
```
### int\double 转字符串
``` dart
int age = int.parse("12");
double price = double.parse("5.28");
String ageStr = age.toString();
String priceStr = price.toString();
```
### int\double 转字符串保留精确度
``` dart
int age = 12;
double pi = 3.1415926;
print(age.toStringAsFixed(3));
// 12.000
print(pi.toStringAsFixed(3));
// 3.131
```
*未完待续*