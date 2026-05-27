---
title: C文件IO函数
tags: [c, io, file, glibc]
created: 2026-05-22
source: raw/notes/C_C++/c_file_func
---

# C文件IO函数

glibc文件相关函数[^1]。

## fopen/popen/fdopen

```c
FILE *fopen(const char *path, const char *mode);
FILE *popen(const char *command, const char *type);
FILE *fdopen(int fd, const char *mode);
```

- 多次对同一个fd调用fdopen，需要多次fclose[^1]

## fgets

读取一行[^1]。

## sscanf

格式化解析[^1]。

## stat

```c
#include <sys/stat.h>
int stat(const char *path, struct stat *buf);
```

获取文件状态[^1]。

## 相关概念

- [[C字符串函数]]
- [[C网络函数]]
- [[C内存函数]]

[^1]: raw/notes/C_C++/c_file_func