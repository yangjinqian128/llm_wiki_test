---
title: 虚拟地址Cache
tags: [CPU, Cache, 虚拟内存]
created: 2026-05-22
source: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md
---

# 虚拟地址Cache (VIVT Cache)

使用虚拟地址作为索引和标签的cache，访问快但存在同名和重名问题。

## 特点

Virtual Index Virtual Tag (VIVT):
- Index: 虚拟地址的一部分
- Tag: 虚拟地址的一部分
- 无需地址翻译即可访问cache

优点：
- 访问速度快（无需等待TLB翻译）
- 适合小容量、高速度cache

缺点：
- 同名问题：同VA不同PA
- 重名问题：同PA不同VA
- 需额外处理机制

## 同名问题处理

同VA不同时间映射不同PA：
- 方案1: 映射变化时清理cache
- 方案2: 引入ASID区分不同进程

## 重名问题处理

同PA不同VA映射：
- 方案1: 使用页内偏移做index（限制cache容量）
- 方案2: 硬件维护反向映射和同步逻辑

## 其他Cache类型

### VIPT (Virtual Index Physical Tag)

- Index: 虚拟地址
- Tag: 物理地址
- 访问cache与TLB翻译并行
- 避免重名问题（tag匹配PA）
- index受限页内偏移

### PIPT (Physical Index Physical Tag)

- Index: 物理地址
- Tag: 物理地址
- 无同名和重名问题
- 需等待TLB翻译
- 适合大容量cache

## 实际应用

现代CPU：
- L1 cache: VIPT或PIPT（权衡速度和问题）
- L2/L3 cache: PIPT（大容量，问题更严重）
- TLB: VIVT（小容量，直接相连）

## 相关概念

- [[物理地址Cache]]
- [[同名问题]]
- [[重名问题]]
- [[VIPT]]
- [[PIPT]]

[^1]: 来源: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md