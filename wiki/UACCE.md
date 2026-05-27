---
title: UACCE
aliases: [uacce, 加速器框架, User-space Accelerator]
tags: [加速器, uacce, Linux内核]
source: raw/notes/uacce_summary, raw/notes/uacce_fork
---

# UACCE

User-space Accelerator，Linux 内核加速器框架，支持用户态直接访问硬件加速器。

## 概述

UACCE 提供：

- 用户态 mmap MMIO/DUS 接口
- SVA（Shared Virtual Addressing）支持
- 无需内核介入的任务下发

## 核心特性

| 特性 | 说明 |
|------|------|
| MMIO | 控制寄存器映射 |
| DUS | Device User Space，任务描述符区域 |
| SVA | 设备共享进程地址空间 |
| Queue | 任务队列 |

## Fork 功能设计

UACCE 支持 fork 后子进程继承加速器资源。

### 需要解决的问题

1. Fork 队列：子进程需要新队列
2. 重建 MMIO/DUS 映射
3. 新队列与子进程地址空间 SVA bind

### O_FORK 属性

新增文件属性，fork 时为文件创建新 struct file。

### 实现方式

| 方式 | 说明 |
|------|------|
| vma_open | fork 后重建映射（去掉 VM_DONTCOPY） |
| vma_fault | 缺页时重建映射（保留 VM_WIPEONFORK） |

### 数据同步

mmio/dus 内容从父进程拷贝到子进程，避免子进程初始状态错误。

约束：不能在收发包过程中 fork。

## 队列模型

参见[[硬件队列设计]]。

## 相关概念

- [[UADK]] - UADK 软件栈
- [[UACCE fork]] - fork 功能设计
- [[SVA]] - Shared Virtual Addressing
- [[PASID]] - Process Address Space ID
- [[IOMMU]] - 地址翻译

---

**参考来源**：[uacce_summary](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/uacce_summary), [uacce_fork](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/uacce_fork)