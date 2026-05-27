---
type: knowledge-node
tags: [core-concept, auto-generated, system-programming]
last_compiled: 2026-05-22
---

# inode

[[inode]]是文件系统的索引节点[^1]。

## 存储内容

存放文件属性[^1]：
- 文件大小[^1]
- 所有者[^1]
- 所有组[^1]
- 权限[^1]
- 时间戳[^1]

## stat结构

`struct stat`从inode节点得到内容[^1]。

```
int stat(const char *pathname, struct stat *buf);
```
[^1]

## 文件系统结构

每个柱面包含[^1]：
- 超级块副本[^1]
- 配置信息[^1]
- i节点图[^1]
- 块位图[^1]
- i节点[^1]
- 数据块[^1]

## 硬链接与软链接

**硬链接**：只有一个inode节点[^1]。

**软链接**：文件本身inode和软链接inode不同[^1]。

## 创建软链接

```
ln -s A B  /* A <-- B */
```
[^1]

## 相关概念

- [[文件系统]] - 文件系统基础
- [[文件描述符]] - 文件索引
- [[硬链接]] - 硬链接概念

[^1]: 来源：raw/notes/APUE/APUE_note_4