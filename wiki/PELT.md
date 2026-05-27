---
title: PELT
aliases: [Per-Entity Load Tracking, per-entity load tracking]
tags: [调度器, Linux内核]
---

# PELT

PELT(Per-Entity Load Tracking)是 Linux v3.8 引入的调度实体负载跟踪机制。

## 负载定义

```
load = runnable_time / total_time * 1024
```

计算密集任务 load ≈ 1024，多个计算任务 load ≈ n*1024。

## 衰减算法

统计过去 1024us period 的加权累计：
```
load_sum = v0 + v1*y^1 + v2*y^2 + ... + vn*y^n
```
其中 y^32 ≈ 0.5，最大值约 47742。

## 数据结构

```c
struct sched_avg {
    u64 load_sum;      // 加权累计负载
    u64 runnable_sum;  // 加权累计 runnable
    u64 util_sum;      // 加权累计利用率
    u32 load_avg;      // 平均负载
    u32 runnable_avg;  // 平均 runnable
    u32 util_avg;      // 平均利用率
};
```

## 更新点

`update_load_avg` 在各种调度点调用：
- enqueue/dequeue task
- tick 中断
- migration

## 计算示例

计算密集任务(死循环)的 `/proc/PID/sched`:
```
se.avg.load_sum    : 47719
se.avg.load_avg    : 1023
se.avg.util_avg    : 940
```

## period_contrib

记录当前 period 内的多余时间，用于连续计算。

## cfs_rq 负载

CFS 运行队列负载 = 各 se 负载之和，同样使用衰减加权。

## 相关概念

- [[负载均衡]]: 基于 PELT 数据决策
- [[调度域]]: 负载均衡范围
- [[组调度]]: 组也有 PELT 统计

[^1]: 来源: raw/notes/linux/sched/Linux内核调度中负载的计算