---
title: CPU带宽控制
aliases: [CPU bandwidth control, CFS bandwidth control]
tags: [调度器, Linux内核, cgroup]
---

# CPU带宽控制

CPU 带宽控制通过 cgroup 限制线程组的 CPU 使用时间。

## cgroup 接口

`/sys/fs/cgroup/cpu/`
```
cpu.cfs_period_us  : 100000 (100ms)
cpu.cfs_quota_us   : -1 (无限制)
```

`quota` 在 `period` 内允许的最大 CPU 时间。

## 使用示例

```bash
# 创建测试组
mkdir /sys/fs/cgroup/cpu/my_test
# 将 PID 写入
echo $PID > tasks
# 限制为 50% CPU
echo 50000 > cpu.cfs_quota_us  # period的一半
```

死循环程序 CPU 占用从 100% 降至约 50%。

## 实现机制

- `cfs_rq->runtime_remaining`: 剩余配额
- `cfs_rq->throttled`: 被限制状态
- 定期补充配额: `unthrottle_cfs_rqs`

## 关键函数

- `assign_cfs_rq_runtime`: 分配配额
- `check_cfs_rq_runtime`: 检查是否超限
- `throttle_cfs_rq`: 限制运行队列

## throttle 流程

```
pick_next_task_fair
  -> check_cfs_rq_runtime(cfs_rq)
    if runtime <= 0
      -> throttle_cfs_rq
```

## 与 shares 区别

- `cpu.shares`: 相对权重分配
- `cpu.cfs_quota_us`: 绝对时间限制

## 相关概念

- [[组调度]]: 带宽控制作用于组
- [[PELT]]: 统计负载用于决策
- [[负载均衡]]: 被 throttle 的组不参与均衡

[^1]: 来源: raw/notes/linux/sched/Linux系统CPU带宽控制