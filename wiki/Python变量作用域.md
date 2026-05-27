---
title: Python变量作用域
tags: [python, scope, global, local]
created: 2026-05-22
source: raw/notes/python/python_note.md
---

# Python变量作用域

Python中变量的作用域分为local和global[^1]。

## global关键字

```python
global y
y = 3

def func_1():
    global y  # 指示y是全局变量
    y = 5
    print "y is", y

func_1()
print "y is", y  # 输出5，全局y被修改
```

函数内的变量默认是local的，不改变外部同名变量[^1]。

## 数据存储方式

Python数据分为可变和不可变变量[^1]：

- **不可变**: 数字、字符串
- **可变**: 列表、字典、集合

```python
x = 3
y = x  # y是x的索引，id(x) == id(y)

x = 5  # x重新创建，id改变，y不变
```

## 相关概念

- [[Python数据结构]]
- [[Python函数参数]]
- [[Python面向对象]]

[^1]: raw/notes/python/python_note.md