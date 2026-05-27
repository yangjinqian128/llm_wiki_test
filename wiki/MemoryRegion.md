---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, memory-management]
last_compiled: 2026-05-22
---

# MemoryRegion

MemoryRegion 是 QEMU 内存系统的基本单元，描述一段地址范围的属性和访问方式[^1]。

## 子类型

| 类型 | 描述 |
|------|------|
| **RAM MemoryRegion** | 有实际 host 端内存 ([[RAMBlock]]) 作为后背 |
| **MMIO MemoryRegion** | 读写时回调 ops 里的 read/write 函数 |
| **ROM MemoryRegion** | 只读 |
| **Alias MemoryRegion** | 另一个 MemoryRegion 的"窗口" |
| **Container MemoryRegion** | 纯粹装子 region 的容器 |

## 树状结构

通过 `memory_region_add_subregion()` 组装成树[^1]：

- 子 region 之间有优先级 (priority)
- 同优先级下后添加的覆盖先添加的

## 关键字段

```c
struct MemoryRegion {
    hwaddr addr, size;
    RAMBlock *ram_block;
    const MemoryRegionOps *ops;
    QTAILQ_HEAD(, MemoryRegion) subregions;
};
```

## 与其他概念的关系

- 展平后形成 [[FlatView]][^1]
- 被 [[MemoryListener]] 监听变化[^1]
- 在 [[AddressSpace]] 中组织[^1]

## 相关概念

- [[FlatView]]
- [[RAMBlock]]
- [[MemoryListener]]
- [[AddressSpace]]

[^1]: 来源: raw/notes/ai_gen/QEMU内存管理基本逻辑.md