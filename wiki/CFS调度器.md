---
title: CFS调度器
aliases: [Completely Fair Scheduler, CFS]
tags: [调度器, Linux内核]
---

# CFS调度器

CFS 是 Linux 默认的调度类，实现完全公平调度。

## 核心思想

找到运行时间最少的线程优先调度：
- IO 密集线程运行时间少，优先调度
- 计算密集线程公平分配时间

## vruntime

虚拟运行时间考虑优先级：
```
vruntime += delta_exec * (NICE_0_LOAD / weight)
```

高优先级 weight 大，vruntime 增长慢，获得更多实际时间。

## 数据结构

```c
struct cfs_rq {
    struct rb_root tasks_timeline;  // 红黑树
    struct rb_node *rb_leftmost;    // 最小vruntime节点
    u64 min_vruntime;               // 最小vruntime
};
```

## 挑选任务

```
pick_next_task_fair
  -> pick_next_entity
    // 取 rb_leftmost
```

## 更新vruntime

```
update_curr(cfs_rq)
  delta_exec = now - exec_start
  vruntime += calc_delta_fair(delta_exec, curr)
```

## 调度类优先级

- dl_sched_class (最高)
- rt_sched_class
- fair_sched_class (CFS)
- idle_sched_class (最低)

## min_granularity

`sysctl_sched_base_slice`: 每个任务最小运行时间，防止调度过于频繁。

## 相关概念

- [[PELT]]: 负载跟踪
- [[组调度]]: CFS 支持组调度
- [[负载均衡]]: CFS 任务参与均衡

[^1]: 来源: raw/notes/linux/sched/linux_scheduler