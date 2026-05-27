---
type: knowledge-node
tags: [core-concept, auto-generated, memory-protection]
last_compiled: 2026-05-22
---

# mprotect

[[mprotect]] 是 Linux 系统调用，用于改变一段内存的读写属性，常用于定位内存踩踏问题[^1]。

## 功能原理

当非法访问被保护的内存时，系统向进程发送 [[SIGSEGV]] 信号[^1]。

进程可在信号处理函数中调用 [[backtrace]] 打印调用栈定位问题[^1]。

## 参数要求

参数包括：虚拟地址、大小（必须页对齐）、保护属性[^1]。

保护属性有：`PROT_READ`、`PROT_WRITE`、`PROT_EXEC`、`PROT_NONE`[^1]。

## 使用场景

对于只读被踩内存：直接用 [[mprotect]] 保护[^1]。

对于可读写被踩内存：正常执行时设置为读写，完成后设置为只读[^1]。

## ARM64 注意事项

ARM64 环境下需使用 `gcc -rdynamic -funwind-tables` 编译，否则 [[backtrace]] 只能打出模块名[^1]。

[^1]: 来源：raw/notes/mprotect_note.txt