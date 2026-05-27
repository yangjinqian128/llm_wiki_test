---
title: Python面向对象
tags: [python, oop, class, inheritance]
created: 2026-05-22
source: raw/notes/python/python_note.md
---

# Python面向对象

Python支持类、继承、多态等面向对象特性[^1]。

## 类定义

```python
class ball(object):
    def __init__(self, r, color="green", vendor="A"):
        self.r = r
        self.color = color
        self.__vendor = vendor  # 私有变量
    
    def run(self):
        print "ball is running"
    
    def show_color(self):
        print "color:", self.color

ball_test = ball(5)
ball_test.show_color()
```

私有变量用`__`前缀，实际可通过`_ball__vendor`访问[^1]。

## 继承

```python
class football(ball):
    def run(self):
        print "football is running"

football_test = football(10)
football_test.run()  # 输出 "football is running"
```

## 多态

```python
def run(ball_t):
    ball_t.run()

run(ball_test)      # "ball is running"
run(football_test)  # "football is running"
```

## 特点

- 不需要提前定义变量[^1]
- 函数名是函数对象的引用[^1]
- `raw_input()`返回字符串，需要`int(raw_input())`转换[^1]

## 相关概念

- [[Python变量作用域]]
- [[Python函数参数]]
- [[Python数据结构]]

[^1]: raw/notes/python/python_note.md