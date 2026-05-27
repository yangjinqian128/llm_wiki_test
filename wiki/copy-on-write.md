---
type: knowledge-node
tags: [core-concept, auto-generated, memory-optimization, process]
last_compiled: 2026-05-22
---

copy-on-write(COW)是内存优化技术，延迟复制直到实际写入时才发生。

[[fork]]中的应用[^1]：
- 子进程复制父进程的页表
- 页表项标记为只读
- 共享物理页

写操作触发COW[^1]：
- 进程尝试写入只读页
- 触发缺页异常
- 内核复制物理页
- 更新页表为可写

优化效果：
- fork无需立即复制所有内存
- 只复制实际修改的页
- 大幅降低fork开销

条件[^1]：
- 不配置VM_WIPEONFORK标志
- 默认启用COW机制

COW是[[进程创建]]性能优化的关键技术。

相关概念包括[[fork]]、[[进程创建]]、[[内存管理]]。

[^1]: 来源于原始素材 raw/notes/fork_note 第26-28行。