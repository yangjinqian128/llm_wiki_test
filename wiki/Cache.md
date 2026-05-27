---
title: Cache
aliases: [缓存, CPU缓存]
tags: [hardware, cpu, cache]
source: raw/notes/cpu硬件/cache_note
---

# Cache

CPU缓存用于弥补CPU和内存间的性能差距。

## 基本结构

```
Cache Set (组)
+---+-----+-----------------------------------+
| v | tag | cache line                        |
+---+-----+-----------------------------------+
| v | tag | cache line                        |
+---+-----+-----------------------------------+
...
```

- Cache分为多个set（组）
- 每个set包含多个cache line（路数way）
- 每个cache line包含valid、tag和数据

## 地址映射

```
| tag(m-s-b) | index(s bit) | offset(b bit) |
```

- index：定位set
- tag：匹配cache line
- offset：定位字节

## 映射类型

| 类型 | set数 | way数 | 特点 |
|------|-------|-------|------|
| 直接映射 | 1 | N | 固定位置，冲突率高 |
| 全相连 | N | 1 | 灵活但查找慢 |
| 组相连 | S | W | 平衡方案，主流 |

## 实例：32KB 4路组相连

- cache line = 32B → offset = 5bit
- set数 = 32KB / (32 * 4) = 256 → index = 8bit
- tag = 32 - 8 - 5 = 19bit

## Cache颠簸

全相连cache中，不同地址映射相同cache line，频繁替换导致性能下降。组相连通过多路减少此问题。

## 相关概念

### 缓存组织
- [[cache line]] - 缓存行（基本单位）
- [[Way]] - 缓存路数
- [[Cache set]] - 缓存组
- [[Cache映射模式]] - 映射策略

### 地址映射
- [[VIPT]] - 虚拟索引物理标记
- [[PIPT]] - 物理索引物理标记
- [[虚拟地址Cache]] - 虚拟地址缓存

### 一致性
- [[MESI协议]] - 缓存一致性协议
- [[cache一致性]] - 多核同步机制
- [[Cache清理]] - Cache maintenance

### 性能
- [[TLB]] - 页表缓存
- [[分支预测]] - 减少流水线停顿