---
type: knowledge-node
tags: [core-concept, auto-generated, Linux, DMA, synchronization]
last_compiled: 2026-05-22
---

# dma_fence

dma_fence 是 Linux DMA 完成同步原语，用于跟踪 DMA 操作的完成状态[^1]。

## 核心用途

在 [[dma-buf]] 体系中用于[^1]：

- 确保 DMA 操作完成后再进行后续访问
- 隐式同步：poll 等待 fence 完成
- 显式同步：通过 sync_file 封装传递给用户态

## fence 类型

在 [[dma_resv]] 对象中管理三种 fence[^1]：

| 类型 | 用法 | 含义 |
|------|------|------|
| 共享 fence | `DMA_RESV_USAGE_READ` | 读操作，允许多个并发 |
| 排他 fence | `DMA_RESV_USAGE_WRITE` | 写操作，与其他互斥 |
| KERNEL fence | `DMA_RESV_USAGE_KERNEL` | 内核内部使用 |

## poll 等待

`dma_buf_poll()` 注册 fence 回调[^1]：

- `POLLOUT`: 等待所有 fence（包括共享和排他）
- `POLLIN`: 只等待最近的排他 fence

## sync_file

sync_file 将 fence 包装为文件描述符[^1]：

- `DMA_BUF_IOCTL_EXPORT_SYNC_FILE`: 导出 fence 为 sync_file
- `DMA_BUF_IOCTL_IMPORT_SYNC_FILE`: 将 sync_file fence 注入 dma-buf

## 相关概念

- [[dma-buf]]
- [[dma_resv]]
- [[VFIO]]

[^1]: 来源: raw/notes/ai_gen/dma-buf-analysis.md