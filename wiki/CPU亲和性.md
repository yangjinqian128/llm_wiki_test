---
title: CPU亲和性
aliases: [CPU Affinity, 进程绑定]
tags: [cpu, scheduling, affinity]
---

# CPU亲和性

进程或线程绑定到特定CPU运行的机制。

## 系统调用

- `sched_setaffinity` - 设置亲和性
- `sched_getaffinity` - 获取亲和性

## 用户工具

- [[taskset]] - 简单CPU绑定工具
- [[numactl]] - NUMA综合控制工具

## 相关概念

- [[NUMA-balancing]] - 自动内存迁移
- [[NUMA]] - NUMA架构
- [[内核线程]] - 线程调度