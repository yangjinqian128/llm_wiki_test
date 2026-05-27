---
title: TLB
aliases: [Translation Lookaside Buffer, 地址翻译缓存]
tags: [hardware, mmu, cache]
source: raw/notes/cpu硬件/tlb_note
---

# TLB

MMU页表的缓存，加速虚拟地址到物理地址的翻译。

## TLB类型

| 类型 | 作用 |
|------|------|
| Stage1 TLB | VA→IPA翻译缓存 |
| Combine TLB | Stage1+Stage2综合结果 |
| Stage2 TLB | IPA→PA翻译缓存 |

虚拟化场景下，Stage2 TLB避免二次page table walk，降低TLB miss成本。

## 覆盖范围

TLB entry可覆盖：
- 基础页（4KB等）
- Block页（大块映射）
- CONT PTE（连续页表项）

## 大页优化

使用[[大页]]可减少 TLB 条目数量，降低 TLB miss：
- 2MB 大页：覆盖范围是 4KB 页的 512 倍
- 1GB 大页：覆盖范围是 4KB 页的 262144 倍

相关技术：
- [[透明大页]] - 自动合并为大页
- [[hugetlbfs]] - 大页文件系统

## 相关概念

- [[MMU]] - 内存管理单元
- [[页表]] - 地址翻译表
- [[Cache]] - 数据缓存
- [[虚拟化]] - Stage2翻译
- [[大页]] - 减少TLB压力