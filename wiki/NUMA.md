---
title: NUMA
aliases: [Non-Uniform Memory Access]
tags: [architecture, memory, numa]
---

# NUMA

非统一内存访问架构，多核系统中不同CPU访问不同内存区域延迟不同。

## NUMA节点

每个NUMA节点包含一组CPU和本地内存，访问本地内存速度快，跨节点访问慢。

## 控制机制

- [[numactl]] - 手动NUMA控制
- [[NUMA-balancing]] - 自动内存迁移
- [[CPU亲和性]] - 进程绑定到NUMA节点

## 相关概念

- [[内存迁移]] - 页面迁移机制
- [[内存策略]] - 内存分配策略
- [[内存管理]] - NUMA内存管理