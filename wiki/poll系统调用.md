---
title: poll系统调用
aliases: [poll, select, IO多路复用]
tags: [kernel, io, syscall]
source: raw/notes/poll_note
---

# poll系统调用

Linux内核poll/select系统调用的实现机制，用于IO多路复用。

## 主流程

```c
SYSCALL_DEFINE3(poll, ...)
  → do_sys_poll
    → do_poll(head, &table, end_time)
      for (;;) {
          for (;;) {
              do_pollfd()
                → vfs_poll()
                  → file->f_op->poll()  // 驱动poll回调
          }
          if (count || timed_out) break;
          poll_schedule_timeout();  // 等待唤醒
      }
```

## 驱动poll回调

典型实现（如UACCE驱动）：

```c
static __poll_t uacce_fops_poll(struct file *file, poll_table *wait)
{
    poll_wait(file, &q->wait, wait);  // 注册等待队列
    if (uacce->ops->is_q_updated && uacce->ops->is_q_updated(q))
        return EPOLLIN | EPOLLRDNORM;
    return 0;
}
```

## 执行流程

1. 循环调用各fd的底层poll回调
2. poll_wait将自己挂到等待队列（仅添加entry，立即返回）
3. 检测是否有数据ready
4. 如无事件且未超时，进入睡眠等待
5. 中断唤醒后重新poll检查

## 相关概念

- [[IO多路复用]] - epoll/select/poll对比
- [[等待队列]] - 内核等待机制
- [[中断处理]] - 硬件完成唤醒
- [[UACCE]] - 加速器框架poll实现