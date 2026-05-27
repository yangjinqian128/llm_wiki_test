---
title: Cache映射模式
tags: [CPU, Cache, 计算机体系结构]
created: 2026-05-22
source: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md, raw/notes/cpu硬件/cache_note.md
---

# Cache映射模式

内存地址到cache位置的映射方式，分为直接相连、全相连和组相连三种。

## 直接相连 (Direct Mapped)

一个地址上的数据只能放到cache的一个固定位置：
- 地址映射到唯一cache line
- 简单高效，硬件实现简单
- 易发生cache颠簸

缺点：
- 不同地址映射到同一位置
- 频繁替换，性能下降

典型应用：TLB（小容量，直接相连可行）

## 全相连 (Fully Associative)

一个地址上的数据可以放到cache的全部cache line上：
- 需遍历整个cache查找
- 灵活性最高，替换算法空间大
- Cache大时效率差

缺点：
- Cache颠簸风险
- 需比较全部tag
- 硬件复杂度高

## 组相连 (Set Associative)

Cache分成多个组(set)，一个地址上的数据只可放到一个组，但组内有多个cache line(way)：
- 组内全相连，组间直接映射
- 平衡灵活性和效率
- 现代CPU主流方案

示例：
```
                           way
       +------------+------------+------------+-------------+
 set0   | cache line | cache line | cache line | cache line  |
       +------------+------------+------------+-------------+
 set1   | cache line | cache line | cache line | cache line  |
       +------------+------------+------------+-------------+
 ...
 setN   | cache line | cache line | cache line | cache line  |
       +------------+------------+------------+-------------+
```

4路组相连cache，每个set有4个cache line可选。

## 地址划分

内存地址划分为：
- **tag**: 高位，用于匹配cache line
- **index**: 中位，选择set
- **offset**: 低位，选择cache line内字节

示例：
32KB cache，64B cache line，4路：
- offset: 6 bit (64B)
- index: 7 bit (128 set)
- tag: 其余高位

## 路数选择

4KB页面下，使用虚拟地址作为index的最小路数：

| Cache大小 | 最小路数 |
|----------|---------|
| 8KB      | 2 way   |
| 16KB     | 4 way   |
| 32KB     | 8 way   |
| 64KB     | 16 way  |
| 128KB    | 32 way  |

原因：index+offset不能超过12 bit页内偏移。

## Cache颠簸

全相连cache的问题：
```
           +------------+              +------------+
        A  | ddr        |  --------->  | cache line |
           +------------+         /    +------------+
        B  | ddr        |  ------/-->  | cache line |
           +------------+       
```
A和B地址映射到同一cache line，交替访问导致频繁替换。

组相连可缓解：同一set有多个way，可同时cache A和B。

## 相关概念

- [[Cache]]
- [[Cache line]]
- [[Cache set]]
- [[Way]]
- [[TLB]]

[^1]: 来源: raw/notes/cpu硬件/CPU中cache和MMU的基本逻辑.md, raw/notes/cpu硬件/cache_note.md