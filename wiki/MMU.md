---
title: MMU
aliases: [Memory Management Unit, 内存管理单元]
tags: [hardware, memory, mmu]
---

# MMU

内存管理单元，负责虚拟地址到物理地址的翻译。

## 地址翻译

MMU通过页表将虚拟地址翻译为物理地址，TLB作为页表缓存加速翻译过程。

## 相关概念

- [[TLB]] - 地址翻译缓存
- [[页表]] - 地址翻译表结构
- [[Cache]] - 数据缓存与MMU交互
- [[Load-Store单元]] - LSU与MMU协作
- [[虚拟化]] - Stage2 MMU翻译