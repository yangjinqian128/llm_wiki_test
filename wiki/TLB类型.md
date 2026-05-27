---
title: TLB类型
tags: [CPU, TLB, MMU, 虚拟化]
created: 2026-05-22
source: raw/notes/cpu硬件/tlb_note.md
---

# TLB类型

TLB在硬件内部的多种保存形式，包括stage1 TLB、combine TLB和stage2 TLB。

## Stage1 TLB

保存VA->IPA（中间物理地址）映射：
- 非虚拟化场景：VA->PA
- 虚拟化场景：Guest VA->Guest PA (IPA)
- 第一阶段地址翻译的缓存

## Combine TLB

综合stage1和stage2得到的TLB：
- 保存VA->PA的最终映射
- 一次命中完成完整地址翻译
- 省去stage2 page table walk

优点：
- 地址翻译效率高
- 单次查找完成翻译

缺点：
- Entry数量可能受限
- 虚拟化场景需维护更多映射

## Stage2 TLB

保存IPA->PA映射：
- 虚拟化场景专用
- Guest PA到Host PA的翻译缓存
- 与stage1 TLB配合使用

优点：
- Stage1 TLB miss时可立即获取IPA对应的PA
- 无需额外stage2 page table walk
- 减少TLB miss成本

## 虚拟化场景

只有combine TLB时：
- Stage1 TLB miss需做stage1 page table walk
- 得到IPA后还需做stage2 page table walk
- TLB miss成本高

有stage2 TLB时：
- Stage1 TLB miss做stage1 page table walk
- 得到IPA后查stage2 TLB
- 可立即得到PA，省去stage2 walk

## TLB覆盖范围

从一条TLB覆盖范围看：
1. 基础页大小（如4KB）
2. Block页大小（如1GB block）
3. CONT PTE大小（如64KB contiguous）

不同覆盖范围：
- 大页TLB entry覆盖更大地址范围
- 减少TLB entry数量需求
- 提高TLB命中率

## 相关概念

- [[TLB]]
- [[Stage2翻译]]
- [[VMID]]
- [[大页]]
- [[Contiguous PTE]]

[^1]: 来源: raw/notes/cpu硬件/tlb_note.md