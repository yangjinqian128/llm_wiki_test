---
title: Cache line
tags: [CPU, Cache, 计算机体系结构]
created: 2026-05-22
source: raw/notes/cpu硬件/cache_note.md
---

# Cache line

Cache的基本存储单位，包含有效位、标签和数据块。

## 结构

每个cache line包含：
- **v (valid)**: 有效位，指示数据是否有效
- **tag**: 标签，用于地址匹配
- **data**: 数据块，存储实际数据

示意图：
```
+---+-----+-----------------------------------+
| v | tag | cache line (data)                 |
+---+-----+-----------------------------------+
```

## 大小

常见cache line大小：
- 32 Byte
- 64 Byte（主流）
- 128 Byte

大小影响：
- 越大：空间利用率可能降低（加载未用数据）
- 越小：管理开销大，地址映射复杂

## 与内存地址关系

内存地址划分：
```
         tag(m-s-b)       index(s bit)        offset(b bit)
       |            |                      |                |
       |<---------->|<-------------------->|<-------------->|
```

- **offset**: 选择cache line内具体字节
- offset位数 = log2(cache line大小)

示例：
- 64 Byte cache line: offset = 6 bit
- 32 Byte cache line: offset = 5 bit

## Cache set

Cache分成多个set：
- 每个set包含一定数目cache line
- cache line数 = way数
- 地址index域选择set

## 替换

当cache line需替换时：
- 根据替换算法选择牺牲cache line
- 常见算法：LRU、随机、PLRU等
- 若牺牲cache line是dirty，需写回内存

## 相关概念

- [[Cache]]
- [[Cache set]]
- [[Way]]
- [[Cache映射模式]]

[^1]: 来源: raw/notes/cpu硬件/cache_note.md