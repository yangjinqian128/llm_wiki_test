---
type: knowledge-node
tags: [core-concept, auto-generated, accelerator]
last_compiled: 2026-05-22
---

# UACCE fork

[[UACCE fork]] 使子进程无需重新申请加速器资源即可使用[^1]。

## 需求背景

进程拥有 CPU 和加速器两种计算资源[^1]。

新进程使用加速器需重新 open 设备并初始化队列[^1]。

[[UACCE fork]] 后子进程 mmio/dus 可直接使用[^1]。

## 三个问题

1. **fork 队列**：内核为子进程申请新队列[^1]
2. **重建 map**：重建 mmio/dus 的映射[^1]
3. **地址空间绑定**：新队列与子进程 mm 做 [[SVA]] bind[^1]

## O_FORK 属性

给文件添加 `O_FORK` 属性[^1]。

语义是 fork 时为文件在子进程创建新上下文[^1]。

在 `fork->copy_files` 中判断并创建新 `struct file`[^1]。

## vma 处理

两种方式[^1]：
- **vma_open**：去掉 `VM_DONTCOPY`[^1]
- **vma_fault**：去掉 `VM_DONTCOPY`，保留 `VM_WIPEONFORK`[^1]

## 数据同步问题

mmio/dus 内容需从父进程拷贝到子进程[^1]。

拷贝必须在子进程 fd 与地址空间 bind 之后[^1]。

否则触发计算任务会出错[^1]。

[^1]: 来源：raw/notes/uacce_fork