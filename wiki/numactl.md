---
title: numactl
aliases: [NUMA控制工具]
tags: [tool, numa, cpu]
source: raw/notes/taskset_numactl
---

# numactl

libnuma库提供的用户态工具，用于查看和控制NUMA相关配置。

## 功能

### CPU绑定

- `--cpubind=1` - 绑定到NUMA节点1的CPU
- `-C A-B` - 绑定到CPU A-B
- 使用`sched_setaffinity`系统调用实现

### 内存绑定

- `--membind=1` - 绑定内存到NUMA节点1
- 使用`set_mempolicy`系统调用实现
- 绑定后消除缺页中断（内存已预分配）

## 与taskset对比

| 工具 | CPU绑定 | 内存绑定 | 系统调用 |
|------|---------|----------|----------|
| taskset | 支持 | 不支持 | sched_setaffinity |
| numactl | 支持 | 支持 | sched_setaffinity + set_mempolicy |

## 相关概念

- [[taskset]] - 简单CPU亲和性工具
- [[NUMA-balancing]] - 自动NUMA平衡
- [[CPU亲和性]] - 进程CPU绑定
- [[内存策略]] - 内存分配策略