---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, memory-management]
last_compiled: 2026-05-22
---

# MemoryListener

MemoryListener 是 QEMU 内存拓扑变化的观察者模式实现[^1]。

## 核心回调

| 回调 | 作用 |
|------|------|
| `begin()` / `commit()` | 事务生命周期 |
| `region_add()` | 新 range 出现 |
| `region_del()` | range 删除 |
| `log_start()` / `log_stop()` | dirty logging 状态变化 |
| `log_sync()` | 读取 dirty bitmap |

## Priority

数字越小，forward 方向 (add/start) 越先被调用，reverse 方向 (del/stop) 越后被调用[^1]。

## 实现示例

**Virtio**：只关心 `commit` 回调刷新 virtqueue ring 地址缓存[^1]

**VFIO**：完整实现[^1]
- `region_add` → `VFIO_IOMMU_MAP_DMA`
- `region_del` → `VFIO_IOMMU_UNMAP_DMA`
- `log_sync` → 读取 dirty bitmap

## 注册时 Replay

`memory_listener_register()` 时立即重放当前 [[FlatView]] 全部内容[^1]：

```
listener->begin()
遍历所有 FlatRange → listener->region_add()
listener->commit()
```

## 相关概念

- [[MemoryRegion]]
- [[FlatView]]
- [[AddressSpace]]
- [[VFIO]]

[^1]: 来源: raw/notes/ai_gen/QEMU内存管理基本逻辑.md