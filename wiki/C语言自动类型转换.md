---
title: C语言自动类型转换
tags: [c, type, conversion]
created: 2026-05-22
source: raw/notes/C_C++/c_note
---

# C语言自动类型转换

C语言中的数据类型自动转换规则[^1]。

## 关键点

- sizeof()返回unsigned int[^1]
- float类型不能直接比较相等/不等[^1]
- 有符号数和无符号数运算、赋值需注意[^1]
- 有符号右移带符号位[^1]
- 各种截断数据[^1]

## 参考资料

https://akaedu.github.io/book/ch15s03.html[^1]

## 相关概念

- [[C语言宏]]
- [[C语言格式化打印]]

[^1]: raw/notes/C_C++/c_note