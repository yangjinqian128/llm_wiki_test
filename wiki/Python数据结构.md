---
title: Python数据结构
tags: [python, data-structure, list, dict]
created: 2026-05-22
source: raw/notes/python/python_note.md
---

# Python数据结构

Python内置数据结构包括列表、元组、字符串、字典、集合[^1]。

## 列表(List)

```python
shoplist = ['apple', 'mango', 'carrot', 'banana']

shoplist.__len__()     # 长度
shoplist.sort()        # 排序
shoplist.append('pear')  # 添加
shoplist.__delitem__(0)   # 删除索引0
shoplist[-1]           # 最后一个元素
```

列表元素可变[^1]。

## 元组(Tuple)

```python
zoo = ('wolf', 'elephant', 'penguin')
new_zoo = ('monkey', 'dolphin', zoo)

len(zoo)               # 3
new_zoo[2]             # ('wolf', 'elephant', 'penguin')
new_zoo[2][2]          # 'penguin'
```

元组不可变，用于多返回值[^1]。

## 字符串切片

```python
string = "0123456789"
string[0:5]            # "01234"
string[2:]             # "23456789"
string[1:-1]           # "12345678"
string[:]              # 整个字符串
string[::4]            # 每4个取一个
```

## 字典(Dictionary)

```python
b = {'Larry': 'larry@wall.org', 'Sherlock': 'sherlock@gmail.com'}

b['Larry']             # 取值
b['Sherlock'] = "..."  # 添加
b.pop('Larry')         # 删除
'Larry' in b           # 判断存在
```

key必须是不可变变量[^1]。

## 集合(Set)

```python
s = set([1, 2, 3, 4])
s.add(10)              # 添加
s.remove(1)            # 删除
2 in s                 # 判断存在
```

元素必须是不可变变量[^1]。

## 相关概念

- [[Python变量作用域]]
- [[Python函数参数]]

[^1]: raw/notes/python/python_note.md