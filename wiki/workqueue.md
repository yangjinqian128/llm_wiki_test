---
type: knowledge-node
tags: [core-concept, auto-generated, linux-kernel, async-execution]
last_compiled: 2026-05-22
---

workqueue是Linux内核的异步执行机制，允许内核模块将任务延迟到内核线程执行。

Concurrency Managed Workqueue(CMWQ)是现代版本，代码路径：
- kernel/workqueue.c
- include/linux/workqueue.h[^1]

核心概念[^1]：
- **work**：待执行的任务(函数)
- **workqueue**：任务队列
- **worker pool**：线程池
- **worker**：执行线程

前端(workqueue)与后端(worker pool)解耦[^1]。

类型[^2]：
- **per-cpu workqueue**：系统预分配，绑定CPU
- **unbound workqueue**：用户创建，NUMA节点调度

API使用[^3]：
- `schedule_work(work)`：放到当前CPU
- `queue_work_on(cpu, wq, work)`：放到指定CPU
- `alloc_workqueue()`：创建unbound workqueue

线程动态管理[^4]：
- pool线程阻塞时自动创建新线程
- idle线程超时后自动销毁
- 每个pool至少保留一个idle线程

测试结论[^5]：
- unbound workqueue只能精确到NUMA节点
- 线程阻塞时pool会创建新worker

[[内核工作队列]]提供了延迟执行的机制。

相关概念包括[[内核线程]]、[[延迟执行]]、[[中断处理]]。

[^1]: 来源于原始素材 raw/notes/workqueue_note 第22-28行。
[^2]: 来源于原始素材 raw/notes/workqueue_note 第32-47行。
[^3]: 来源于原始素材 raw/notes/workqueue_note 第35-47行。
[^4]: 来源于原始素材 raw/notes/workqueue_note 第65-82行。
[^5]: 来源于原始素材 raw/notes/workqueue_note 第143-161行。