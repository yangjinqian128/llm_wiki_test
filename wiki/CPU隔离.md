---
title: CPU隔离
aliases: [isolcpus, nohz_full, CPU isolation]
tags: [调度器, 性能优化, Linux内核]
---

# CPU隔离

CPU 隔离是将特定 CPU 从调度中排除，用于专用任务。

## 内核参数

```
isolcpus=0-3     # 隔离 CPU 0-3
nohz_full=0-3    # 关闭 timer tick
```

## isolcpus

隔离的 CPU 不参与调度，除非线程显式绑定。

绑核方式：
- `taskset` 用户态工具
- `sched_setaffinity` 系统调用
- 内核态 `bind_task`

## nohz_full

在只有单个运行线程时关闭 timer tick，减少中断开销。

- timer 任务迁移到其他 CPU
- 定时器到期时发送 IPI 触发执行

## nohz_idle

idle 时关闭 timer tick，类似 nohz_full。

## 隔离核特点

- 可见一对一绑定的内核线程
- 中断 affinity 可独立配置
- 减少 context switch 开销

## taskset 注意

绑定进程到隔离核组时，所有线程绑定到组内一个核。

## 应用场景

- DPDK 网络处理
- 高性能计算
- 实时任务

## 相关概念

- [[负载均衡]]: 隔离核不参与
- [[IPI]]: 定时器到期触发
- [[CPU亲和性]]: 绑核机制

[^1]: 来源: raw/notes/linux/isolcpus_nohz