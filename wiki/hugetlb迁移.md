---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, ARM64, hugetlb]
last_compiled: 2026-05-22
---

# hugetlb迁移

hugetlb 大页在虚拟机 [[热迁移]] 中的处理[^1]。

## 核心问题

- 大页不能拆成 4K 小页[^1]
- 需要保持 hugetlb 大页对齐[^1]
- 迁移粒度需按 block_size[^1]

## KVM Stage-2 映射

使用 hugetlbfs backing 时[^1]：

- Stage-2 映射为 PMD block[^1]
- 脏页跟踪时触发拆分[^1]

## virtio-mem-block 方案

预留专用 GPA 区域[^1]：

- 不参与 Guest Buddy 管理[^1]
- 按 block_size 整块迁移[^1]
- 不调用 `vmstate_register_ram()`[^1]

## v6.3 问题

- 迁移效率低[^1]
- Fault 风暴[^1]

## 相关概念

- [[热迁移]]
- [[脏页跟踪]]
- [[Stage2大页拆分]]
- [[virtio-mem-block]]

[^1]: 来源: raw/notes/ai_gen/virtio-mem-block.md, raw/notes/ai_gen/v6.3_VM_Migration_LargePage_Split.md