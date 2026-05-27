---
title: taskset
aliases: [CPU亲和性工具]
tags: [tool, cpu, affinity]
source: raw/notes/taskset_numactl
---

# taskset

Linux命令，用于绑定进程或线程到特定CPU运行。

## 系统调用

使用`sched_getaffinity`和`sched_setaffinity`系统调用实现。

## 注意事项

- 仅绑定CPU，不控制内存
- 进程的各线程继承相同的CPU绑定

## 相关概念

- [[numactl]] - NUMA综合控制工具
- [[CPU亲和性]] - 进程与CPU绑定关系
- [[NUMA-balancing]] - 自动NUMA平衡