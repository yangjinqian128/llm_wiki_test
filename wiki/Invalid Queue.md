---
title: Invalid Queue
tags: [CPU, Cache一致性, MESI]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md
---

# Invalid Queue

CPU接收其他核发来的cache invalid请求时，将请求暂存队列，随后异步处理。

## 功能

多核系统下cache一致性：
- Core A向其他core发invalid cache请求
- Core B收到请求时可能还有其他事务
- Core B把请求存入invalid queue
- Core B回响应给Core A
- Core B后续处理invalid queue中的请求

## 与Store Buffer关系

Invalid queue与store buffer是正交概念：
- Store buffer: 本核的store操作缓存
- Invalid queue: 其他核发来的invalid请求缓存

两者都会导致乱序行为。

## 内存顺序影响

Invalid queue存在使得投机load可能拿到"更加错误"的值：
- 其他核已发invalid请求
- 本核还未处理（请求在invalid queue）
- 本核load拿到旧值

Load barrier需清空invalid queue：
- 执行barrier后的load前，先处理invalid queue中的请求
- 保证load得到正确值

## Store Barrier与Invalid Queue

Store barrier保证：
- 关闭store queue到store buffer通路
- 等待store buffer请求完成（MESI一致性操作）
- 其他core看到一致数据

但store barrier不保证其他core看到顺序：
- 其他core上load可能乱序执行
- 需要load barrier配合

## 经典MP场景

```
core 0:                  core 1:

store X1, [addr1]       load X3, [addr2]
store barrier
store X2, [addr2]       load X4, [addr1]
```

- Store barrier保证core 0两个store顺序
- 但core 1第二条load可能乱序执行
- X4可能是addr1上的旧值

解决方案：core 1两load之间加load barrier。

## 相关概念

- [[Store Buffer]]
- [[MESI协议]]
- [[Cache一致性]]
- [[Memory Barrier]]
- [[Load-Store单元]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑.md