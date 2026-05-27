---
type: knowledge-node
tags: [core-concept, auto-generated, QEMU, virtio, memory]
last_compiled: 2026-05-22
---

# virtio-mem-block

virtio-mem-block 是 QEMU 设备，管理专用大页内存区域和迁移[^1]。

## 设计目标

- VM 地址空间中预留 GPA 专门给特定应用[^1]
- 内存不参与 Guest Buddy 系统管理[^1]
- [[热迁移]]时按 block_size 整块迁移，不拆成 4K[^1]

## 组件架构

| 组件 | 职责 |
|------|------|
| virtio-mem-block | QEMU设备，管理内存和迁移 |
| virt.c 修改 | 预留 GPA 区域 |
| QEMU ACPI | 上报地址空间给 Guest OS |
| Guest 内核驱动 | 映射 GPA 到用户态 |
| Guest 用户态 API | 分配/释放接口 |

## 关键实现

不调用 `vmstate_register_ram()`[^1]：

- 不参与 ram.c 迁移
- 自定义按 block_size 迁移逻辑

## 数据流

```
Stage1: 内存分配
App → request_app_memory(size)
  → Guest Kernel: app驱动
  → hugetlbfs backend (2M aligned)

Stage2: 迁移
源端: 按 2M block 写入迁移流
目的端: 按 2M block 写入 hugetlbfs
```

## 相关概念

- [[热迁移]]
- [[hugetlbfs]]
- [[virtio]]

[^1]: 来源: raw/notes/ai_gen/virtio-mem-block.md