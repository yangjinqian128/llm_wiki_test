---
type: knowledge-node
tags: [core-concept, auto-generated, device-driver]
last_compiled: 2026-05-22
---

# ioctl

[[ioctl]]是设备控制接口[^1]。

## 用户态接口

```
int ioctl(int fd, int cmd, int arg);
```

- fd：文件描述符[^1]
- cmd：命令字[^1]
- arg：参数[^1]

## 命令字定义

使用内核提供的宏[^1]：
- `_IO(type, nr)`：无数据[^1]
- `_IOR(type, nr, size)`：读数据[^1]
- `_IOW(type, nr, size)`：写数据[^1]
- `_IOWR(type, nr, size)`：读写数据[^1]

## 命令字组成

四段组成[^1]：
1. type：魔术字[^1]
2. nr：命令序号[^1]
3. size：数据大小[^1]
4. 方向：读/写[^1]

## 魔术字

参考`Documentation/ioctl/ioctl-number.txt`[^1]。

## 参数传递

- 传变量：unsigned long[^1]
- 传指针：指向数据结构[^1]

## 驱动实现

在file_operations中实现ioctl回调[^1]。

## 头文件

用户态需要定义命令字宏[^1]。

数据结构在用户态重新定义[^1]。

## 相关概念

- [[命令码]] - ioctl命令字编码
- [[字符设备]] - 字符设备驱动

[^1]: 来源：raw/notes/LDD/scull_2