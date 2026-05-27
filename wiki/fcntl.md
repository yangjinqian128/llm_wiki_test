---
type: knowledge-node
tags: [core-concept, auto-generated, system-programming]
last_compiled: 2026-05-22
---

# fcntl

[[fcntl]]提供文件描述符标志和文件状态标志的查询和修改[^1]。

## 获取文件描述符标志

```
fd = fcntl(0, F_GETFD);
```
[^1]

## 获取文件状态标志

```
fd = fcntl(0, F_GETFL);
```
[^1]

## 复制文件描述符

```
fcntl(fd, F_DUPFD, fd_min)
```

大于或等于fd_min的文件描述符指向fd[^1]。

相当于复制fd到fd_min[^1]。

## O_APPEND

设置文件状态标志[^1]。

write时在文件末尾写入[^1]。

避免多进程竞争[^1]。

## 文件状态标志

- O_RDONLY：只读[^1]
- O_WRONLY：只写[^1]
- O_RDWR：读写[^1]
- O_APPEND：追加[^1]

## 相关概念

- [[文件描述符]] - 文件索引
- [[文件表]] - 内核文件结构

[^1]: 来源：raw/notes/APUE/APUE_note_3