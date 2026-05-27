---
type: knowledge-node
tags: [core-concept, auto-generated, threading]
last_compiled: 2026-05-22
---

# pthread

[[pthread]] 是 POSIX 线程库，提供线程创建、同步和管理接口[^1]。

## 线程 ID

- `getpid()` 返回线程组 ID（主线程内核 pid）[^1]
- `gettid()` 返回线程的内核 pid[^1]
- `pthread_self()` 返回 pthread 库自定义的线程 ID[^1]

## 创建与退出

- `pthread_create` 使用 [[clone]] 系统调用创建线程[^1]
- 线程函数执行完毕自动退出[^1]
- `pthread_exit` 主动退出[^1]
- `pthread_join` 等待线程结束并获取返回值[^1]

## 线程取消

`pthread_cancel(pthread_t tid)` 通过 `tgkill` 发送 `SIGRTMIN` 信号[^1]。

非同步终止，线程可配置：
- `pthread_setcancelstate`：是否响应取消[^1]
- `pthread_setcanceltype`：取消点延迟或立即[^1]

## 同步机制

- **[[mutex]]**：互斥锁[^1]
- **[[spinlock]]**：自旋锁[^1]
- **条件变量**：线程间等待/通知[^1]
- **信号量**：进程/线程同步[^1]

## 线程私有数据

`pthread_key_create` 创建私有数据键[^1]。

`pthread_setspecific`/`pthread_getspecific` 在线程内操作数据[^1]。

[^1]: 来源：raw/notes/pthread_node