---
title: C内存函数
tags: [c, memory, glibc]
created: 2026-05-22
source: raw/notes/C_C++/c_memory_func
---

# C内存函数

glibc内存相关函数[^1]。

## bzero/bcopy

```c
void bzero(void *s, size_t n);
void bcopy(const void *src, void *dest, size_t n);
```

## 共享内存

shmget等共享内存接口[^1]。

## 相关概念

- [[C文件IO函数]]
- [[C字符串函数]]
- [[内存管理]]

[^1]: raw/notes/C_C++/c_memory_func