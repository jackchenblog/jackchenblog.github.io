---
title: 二、Dart常量、变量
date: 2021-05-27 22:27:51
tags: [Flutter, Dart]
---
# 前言
dart是一个强大的脚本类语言，可以不预先定义变量类型，自动会类型推倒
dart中定义变量可以通过var关键字可以通过类型来申明变量
<!--more-->

# 程序入口
main函数为没个类的入口，程序的开始即为main
``` dart
main() {
  
}

// 或
void main() {
  // void 代表函数没有返回值
}
```
# 变量
``` dart
// 定义String类型
// var定义交由dart自动判断类型
var str = "abcd";
// 类型修饰，直接定义此变量类型
String str1 = "abcd";

// 定义int类型
var age = 12;
int yourAge = 12;
```
**!!! var 不可与 int 同时写**

# 常量: final 和 const
const值不变一开始就得赋值
final可以开始不赋值只能赋一次;而final不仅有 const的编译时常量的特性，最重要的它是运行时
永远不改量的量，请使用final或const修饰它，而不是使用var或其他变量类型。
``` dart
// 定义String类型
const name = "Jack";
final name1 = "Bob";

// 定义int
const age = 12;
final age1 = 12;

/*
区别：
const 定义时就需要赋值；
final 可以使用的时候再赋值，类似于懒加载；
*/
// 例如使用final定义一个当前时间，每次运行都会获取最新的时间
final nowTime = new DateTime.now();
print(nowTime);

```

# Dart的命名规则:
```
1、变量名称必须由数字、字母、下划线和美元符($)组成
2.注意:标识符开头不能是数字
3.标识符不能是保留字和关键字。
4.变量的名字是区分大小写的如: age和Age是不同的变量。在实际的运用中，也建议，不要用一个
5、标识符(变量名称)一定要见名思意:变量名称建议用名词，方法名称建议用动词
```