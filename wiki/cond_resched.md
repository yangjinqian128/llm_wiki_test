---
title: cond_resched
aliases: [conditional reschedule]
tags: [调度器, Linux内核]
---

# cond_resched

`cond_resched` 是 Linux 内核主动触发调度的函数。

## 功能

检查是否允许抢占，若允许则主动调度。

## 使用场景

长时间运行的内核代码段，防止 soft lockup：
```c
for (i = 0; i < LARGE_NUMBER; i++) {
    do_heavy_work();
    cond_resched();  // 防止长时间占用
}
```

## 实现逻辑

```c
cond_resched() {
    if (preempt_count == 0 && !irqs_disabled())
        schedule();
}
```

## 注意事项

- 禁止抢占时不会调度
- 关中断时不会调度
- 不可抢占内核中防止 lockup 的关键

## 与 preempt_enable 区别

- `preempt_enable`: 开抢占时可能调度
- `cond_resched`: 显式请求调度点

## 相关概念

- [[内核抢占]]: 被动调度机制
- [[soft-lockup检测]]: 检测长时间占用

[^1]: 来源: raw/notes/linux/linux_cond_resched