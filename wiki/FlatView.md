---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, memory-management]
last_compiled: 2026-05-22
---

# FlatView

FlatView 是 [[MemoryRegion]] 树展平后的结果，形成有序的、互不重叠的 FlatRange 数组[^1]。

## 展平过程

1. 把树递归展开[^1]
2. 解析所有 alias（转化为直接引用）[^1]
3. 按优先级和 overlap 规则切割重叠 region[^1]
4. 排序[^1]

## FlatRange 结构

```c
struct FlatRange {
    MemoryRegion *mr;
    hwaddr offset_in_region;
    AddrRange addr;
    uint8_t dirty_log_mask;
};
```

## RCU 保护

FlatView 是 RCU 保护的[^1]：

- 读写者无锁访问 (`address_space_map`)
- 写者生成新 FlatView 后原子替换 `as->current_map`

## Diff 算法

`address_space_update_topology_pass()` 对新旧 FlatRange 数组做 merge-like 遍历[^1]：

- Range 在旧但不在新 → `region_del`
- Range 完全相同 → `region_nop`
- Range 在新但不在旧 → `region_add`

## 相关概念

- [[MemoryRegion]]
- [[MemoryListener]]
- [[AddressSpace]]

[^1]: 来源: raw/notes/ai_gen/QEMU内存管理基本逻辑.md