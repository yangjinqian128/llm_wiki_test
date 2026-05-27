---
title: DAMON
aliases: [Data Access MONitor, damon]
tags: [内存管理, Linux内核, 监控]
---

# DAMON

DAMON 是 Linux 内核的内存监控子系统，提供主动冷热页管理和内存回收策略。

## 三大功能

1. 内存监控：追踪访问模式
2. 内存回收：主动回收冷页
3. LRU 调整：主动移动冷热页

## 核心创新

将多个 page 组成区域，只用其中一个 page 的 access bit 代表访问情况，减少检测开销。区域会动态合并(不活跃)和拆分(活跃)。

## sysfs 接口

- `/sys/kernel/mm/damon/admin`: 监控配置
- `/sys/module/damon_reclaim`: 内存回收参数
- `/sys/module/damon_lru_sort`: LRU 调整参数

## kdamond 线程

每个业务对应一个 kdamond 内核线程，循环执行：
1. `prepare_access_checks`: 清除 access bit
2. `check_accesses`: 检测访问情况
3. `apply_schemes`: 执行策略(回收/移动)

## 数据结构

- context: 监控上下文
- target: 监控目标(进程)
- region: 内存区域
- scheme: 操作策略

## 用户态工具

`damo`: https://github.com/awslabs/damo

## 相关概念

- [[内存回收]]: DAMON 可主动触发
- [[LRU链表]]: DAMON 调整页面位置
- [[NUMA]]: 支持 NUMA 场景

[^1]: 来源: raw/notes/linux/mm/Linux内存管理-damon