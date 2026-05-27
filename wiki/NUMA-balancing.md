---
title: NUMA Balancing
aliases: [NUMA平衡, 自动内存迁移]
tags: [kernel, numa, memory]
source: raw/notes/numa_balancing
---

# NUMA Balancing

Linux根据程序运行状态自动调整内存位置的特性。当程序从一个NUMA节点的CPU迁移到另一个节点时，系统可自动将内存也迁移到对应NUMA节点。

## 前置条件

需要开启内核配置`CONFIG_NUMA_BALANCING`。

## 控制文件

- `/proc/sys/kernel/numa_balancing` - 启用控制
- `/proc/sys/kernel/numa_balancing_scan_delay_ms` - 扫描延迟
- `/proc/sys/kernel/numa_balancing_scan_period_max_ms` - 最大扫描周期
- `/proc/sys/kernel/numa_balancing_scan_period_min_ms` - 最小扫描周期
- `/proc/sys/kernel/numa_balancing_scan_size_mb` - 扫描大小

## 统计数据

`/proc/vmstat`中`numa_*`相关字段记录NUMA balancing统计数据。

## 相关概念

- [[numactl]] - 手动NUMA控制工具
- [[taskset]] - CPU亲和性设置
- [[内存迁移]] - 页面迁移机制
- [[NUMA]] - Non-Uniform Memory Access架构