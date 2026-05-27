---
title: sysfs文件系统
created: 2026-05-22
tags:
  - Linux
  - sysfs
  - 内核
  - 文件系统
---

# sysfs文件系统

## 概述

sysfs是Linux内核提供的虚拟文件系统，用于导出内核对象信息，便于用户空间读写内核数据。

## 应用场景

驱动调试时实时读写寄存器或变量值。参考示例：`linux/samples/kobject/*`

## 核心数据结构

### kobject

内核对象的核心抽象：

- 几乎所有具体内核对象（如`struct device`）都包含kobject
- 每个kobject在sysfs中对应一个目录

### kset

kobject的集合：

- kset本身也包含一个kobject
- 表示一组相关内核对象

### kobj_type

定义kobject的行为：

- 包含sysfs_ops指针
- 包含attribute指针数组

### attribute

每个attribute对应kobject目录下的一个文件。

### sysfs_ops

定义读写操作：

- **show**：读操作
- **store**：写操作

## 结构关系

```
kobject
  └── kobj_type
        ├── sysfs_ops (show/store)
        └── attribute[] (文件列表)
```

## 使用方法

参考kset-sample.c示例代码。

参见[[Linux内核]]、[[设备驱动]]。

---

**参考来源**：[sysfs_note.md](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/sysfs_note.md)