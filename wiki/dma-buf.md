---
type: knowledge-node
tags: [core-concept, auto-generated, Linux, DMA, memory-sharing]
last_compiled: 2026-05-22
---

# dma-buf

dma-buf 是 Linux 内核跨设备/子系统共享缓冲区的框架，通过文件描述符在进程间传递[^1]。

## 核心场景

- **零拷贝缓冲区共享**: GPU渲染 → DRM显示 → 视频编码[^1]
- **Android ION/DMA-heap 替代**: 从 `/dev/dma_heap/` 分配特定类型内存[^1]
- **VirtIO 虚拟化**: [[udmabuf]] 用于 QEMU/VFIO[^1]
- **PCIe Peer-to-Peer**: 两 PCIe 设备间直接共享[^1]

## 核心数据结构

```
dma_buf
  ├── size: 缓冲区大小（不可变）
  ├── file → struct file (fd 背后)
  ├── ops → struct dma_buf_ops (导出者回调)
  ├── resv → struct dma_resv (fence 管理)
  └── attachments (链表)

dma_buf_attachment
  ├── dmabuf → 指回 dma_buf
  ├── dev → 导入设备
  └── importer_ops → dma_buf_attach_ops
```

## 导出者回调

| 回调 | 必要性 | 作用 |
|------|--------|------|
| `map_dma_buf()` | **必须** | 返回 sg_table |
| `unmap_dma_buf()` | **必须** | 解除 DMA 映射 |
| `release()` | **必须** | 释放底层缓冲区 |
| `mmap()` | 可选 | 支持用户态 mmap |

## 同步机制

| 机制 | 接口 | 适用场景 |
|------|------|---------|
| **隐式同步** | `poll()` + dma_resv fences | OpenGL, 传统媒体驱动 |
| **显式同步** | `EXPORT/IMPORT_SYNC_FILE` | Vulkan, 现代 GPU API |

## 相关概念

- [[DMA]]
- [[VFIO]]
- [[dma_fence]]
- [[dma_resv]]

[^1]: 来源: raw/notes/ai_gen/dma-buf-analysis.md