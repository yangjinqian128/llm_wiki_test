---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, memory]
last_compiled: 2026-05-22
---

# RAMBlock

RAMBlock 是 QEMU 中 RAM 类型 [[MemoryRegion]] 的 host 端实际内存[^1]。

## 关键字段

```c
struct RAMBlock {
    void *host;           // mmap 返回的 HVA
    ram_addr_t offset;    // 在 ram_addr_t 地址空间中的偏移
    int fd;               // -1 (匿名 mmap) 或 hugetlbfs fd
    size_t page_size;     // 4K / 2M / 1G
    MemoryRegion *mr;     // 回指 MemoryRegion
};
```

## 内存分配

```
memory_region_init_ram()
  → qemu_ram_alloc()
    → qemu_ram_mmap()
      → mmap(匿名) 或 mmap(hugetlbfs fd)
      → 返回 HVA
    → rb->host = HVA
```

## 脏页位图

每个 RAMBlock 维护[^1]：

- `rb->bmap`: 主脏页位图
- `rb->clear_bmap`: 辅助位图

## 与 KVMSlot 关系

`KVMSlot.ram` 直接等于 `RAMBlock.host + 偏移`[^1]。

建立 GPA → HVA → HPA 的完整链条[^1]。

## 相关概念

- [[MemoryRegion]]
- [[脏页跟踪]]
- [[热迁移]]

[^1]: 来源: raw/notes/ai_gen/QEMU内存管理基本逻辑.md