---
title: Replay
tags: [CPU, 微架构, 指令执行]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器指令发射的基本逻辑.md
---

# Replay

CPU微架构中的指令重发机制，当投机唤醒导致指令使用错误数据时，重新发射指令执行。

## 触发场景

某些指令执行拍数不确定，如load指令：
- 大部分情况命中cache，拍数可控
- 少量cache不命中情况，拍数长

若投机唤醒（假设命中cache），但实际不命中：
- 依赖load的指令拿到错误数据
- 这些指令需重新执行

## Replay vs Flush

两种恢复方式：
- **Flush**: 清空流水线，重新fetch/decode/rename/dispatch
- **Replay**: 保留指令在issue queue，使用正确数据重新发射

Replay优势：
- 省去重新fetch/decode/rename/dispatch步骤
- 这些步骤对数据输入错误的指令本不需要
- 性能损失更小

Replay劣势：
- 需在issue queue继续保留指令
- 挤占issue queue位置
- 可能导致issue queue变满
- 反压前端rename步骤

## 实现要点

Replay需要：
1. 指令在issue queue保留
2. 正确数据到达后重新唤醒
3. 仲裁逻辑重新发射指令
4. 执行单元使用正确数据执行

对于分支预测出错，仍需flush，因为整个执行路径错误。

## 相关概念

- [[Issue Queue]]
- [[指令发射]]
- [[CPU-Flush]]
- [[投机执行]]
- [[Load-Store单元]]

[^1]: 来源: raw/notes/cpu硬件/超标标量处理器指令发射的基本逻辑.md