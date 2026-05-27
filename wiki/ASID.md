---
title: ASID
aliases: [Address Space Identifier]
tags: [mmu, tlb, context]
---

# ASID

地址空间标识符，用于区分不同进程的TLB entry。

## 用途

避免进程切换时flush全部TLB，仅flush特定ASID的entry。

参见[[TLB]]、[[MMU]]。

## 相关概念

- [[TLB]] - TLB entry标记ASID
- [[MMU]] - 地址空间切换
- [[内存管理]] - 进程地址空间