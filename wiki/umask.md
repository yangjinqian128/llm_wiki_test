---
type: knowledge-node
tags: [core-concept, auto-generated, system-programming]
last_compiled: 2026-05-22
---

# umask

[[umask]]控制文件创建时的默认权限[^1]。

## 默认权限

- 文件：666[^1]
- 目录：777[^1]

## umask作用

去掉对应的读写执行权限[^1]。

## 示例

umask值为022[^1]。

同组用户和其他用户去掉写权限[^1]。

新建文件权限为644[^1]。

## 命令

- `umask`：查看当前值[^1]
- `umask ***`：修改值[^1]

## 相关概念

- [[文件系统]] - 文件权限
- [[inode]] - 文件属性

[^1]: 来源：raw/notes/APUE/APUE_note_4