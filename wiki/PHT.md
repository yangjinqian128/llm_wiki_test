---
title: PHT (Pattern History Table)
tags: [CPU, 分支预测, 微架构]
created: 2026-05-22
source: raw/notes/cpu硬件/超标量处理器分支预测基本逻辑.md
---

# PHT (Pattern History Table)

模式历史表，存储两位饱和计数器，用于分支预测决策。

## 功能

PHT存储预测状态：
- 每个entry是一个两位饱和计数器
- 计数器值决定预测taken还是no taken
- 计数器根据实际跳转结果更新

## 两位饱和计数器

计数器有4个状态：
```
    strong   weak  weak  strong
       no taken     taken
   +-------------------------------+
   |     +-+   +-+   +-+   +-+     |
   |  +--|0|-->|0|-->|1|-->|1|--+  | -> 0: no taken
   |  |  | |   | |   | |   | |  |  |
   |  +->|0|-->|1|-->|0|-->|1|<-+  | -> 1: taken
   |     +-+   +-+   +-+   +-+     |
   +-------------------------------+
```

状态含义：
- **00 (strong no taken)**: 强预测不跳转
- **01 (weak no taken)**: 弱预测不跳转
- **10 (weak taken)**: 弱预测跳转
- **11 (strong taken)**: 强预测跳转

## 状态转移

- strong taken + 实际taken: 保持strong taken
- strong taken + 实际no taken: 移到weak taken（第一次预测失败仍预测taken）
- weak taken + 实际taken: 移到strong taken
- weak taken + 实际no taken: 移到weak no taken（两次都预测失败，改变预测方向）

## 与BHR配合

局部历史预测：
- BHR值作为PHT索引
- 每个BHR状态对应一个PHT entry
- 查表得到预测结果

## 防止意外变化

两位饱和计数器防止零星意外变化：
- 单次预测失败不立即改变预测方向
- 需连续两次失败才改变
- 提高预测稳定性

## 相关概念

- [[BHR]]
- [[两位饱和计数器]]
- [[分支预测]]
- [[局部历史预测]]

[^1]: 来源: raw/notes/cpu硬件/超标量处理器分支预测基本逻辑.md