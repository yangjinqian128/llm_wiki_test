---
title: Load-Store单元
aliases: [LSU, Load Store Unit]
tags: [hardware, cpu, memory]
source: raw/notes/cpu硬件/超标量处理器中访存指令的处理逻辑
---

# Load-Store单元

处理器中处理load/store指令的专用部件（LSU）。

## 地址依赖

| 依赖 | 说明 | 处理 |
|------|------|------|
| RAW | 先写后读相同地址 | Store执行时检测，flush错误load |
| WAR | 先读后写相同地址 | Load不用年轻store buffer值 |
| WAW | 先写后写相同地址 | 顺序执行store消除 |

## 执行策略

- **Store**：顺序执行
- **Load**：投机提前执行（依赖链前端）

## Store Buffer

| 状态 | 说明 |
|------|------|
| no-complete | 地址/数据计算中 |
| complete | 地址/数据ready |
| retired | 已提交，必须写入存储器 |

## RAW违例处理

```
A: store x2, (x3)
B: load x0, (x1)  ← 投机执行
C: add x4, x0, x5  ← 依赖B

A执行时发现B访问相同地址 → flush B及后续
```

## 多核场景

- Store barrier：等待store buffer完成
- Load barrier：清空invalid queue，重新执行load

## 相关概念

- [[Cache]] - LSU与cache交互
- [[MMU]] - LSU与地址翻译
- [[内存屏障]] - barrier语义
- [[Cache一致性]] - MESI协议