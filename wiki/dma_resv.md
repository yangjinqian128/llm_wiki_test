---
type: knowledge-node
tags: [core-concept, auto-generated, Linux, DMA, synchronization]
last_compiled: 2026-05-22
---

# dma_resv

dma_resv (Reservation Object) 是 [[dma-buf]] 框架中管理共享/排他 [[dma_fence]] 的容器[^1]。

## 核心作用

- 管理一组 dma_fence[^1]
- 实现隐式同步机制[^1]
- 被嵌入到 struct dma_buf 中[^1]

## fence 管理

```
dma_resv
  ├── fences[] → fence 数组
  │     ├── fences[WRITE] = 排他 fence
  │     └── fences[READ]  = 共享 fence 列表
  └── seqlock → 保护并发访问
```

## 锁定约定

导入者必须持有 dma_resv 锁时调用[^1]：

- `dma_buf_pin()` / `dma_buf_unpin()`
- `dma_buf_map_attachment()` / `dma_buf_unmap_attachment()`
- `dma_buf_vmap()` / `dma_buf_vunmap()`

导入者不能持有 dma_resv 锁时调用[^1]：

- `dma_buf_attach()` / `dma_buf_detach()`
- `dma_buf_begin_cpu_access()` / `dma_buf_end_cpu_access()`

## 相关概念

- [[dma-buf]]
- [[dma_fence]]
- [[VFIO]]

[^1]: 来源: raw/notes/ai_gen/dma-buf-analysis.md