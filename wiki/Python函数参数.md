---
title: Python函数参数
tags: [python, function, parameter, args]
created: 2026-05-22
source: raw/notes/python/python_note.md
---

# Python函数参数

Python函数支持多种参数类型[^1]。

## 默认参数

```python
def ball(r, color="red", vendor="A", llist=[1, 2, 3]):
    llist.append(4)
    ...

ball(5)                # 使用默认参数
ball(5, "blue")        # 覆盖color
ball(5, vendor="B")    # 指定特定参数
ball(5, llist=[4,5,6]) # 提供新列表
```

**注意**: 默认参数需是不可变参数，否则每次调用会改变默认值[^1]。

## 可变参数

```python
def sum(*number):
    s = 0
    for i in number:
        s += i
    return s

sum(1, 2, 3)           # 直接传入
num = [1, 2, 3, 4]
sum(*num)              # 通过列表传入
```

## 关键字参数

```python
def key_test(a, b, **c):
    print a, b, c

key_test(1, 2)
key_test(1, 2, name="Sherlock")

dict_test = {"name": "John", "age": 12}
key_test(1, 2, **dict_test)  # 通过字典传入
```

## 相关概念

- [[Python变量作用域]]
- [[Python数据结构]]
- [[Python面向对象]]

[^1]: raw/notes/python/python_note.md