---
title: CPU Flush
aliases: [流水线冲刷, 指令刷掉]
tags: [hardware, cpu, pipeline]
source: raw/notes/cpu硬件/CPU微架构里的Flush概念
---

# CPU Flush

超标量处理器中清理投机执行错误指令的机制。

## 触发原因

| 原因 | 说明 |
|------|------|
| 分支预测错 | 刷掉错误路径指令 |
| Load/Store冲突 | RAW违例需刷掉错误load及后续 |
| 异常/中断 | 跳转到异常处理程序 |

## Flush范围

从flush点到allocate位置的所有指令：
- 释放ROB资源
- 释放issue queue资源
- 释放执行单元资源
- 恢复寄存器重命名状态

## 多flush请求处理

同一拍可能产生多个flush请求，硬件从最老flush点开始处理（包含左边各flush）。

## 硬件同步

CPU各部件在时钟信号下同步执行，flush可：
- 立即执行：及时释放资源，需额外检测电路
- 打标记延迟执行：简化实现，但性能较差

## 相关概念

- [[ROB]] - 重排序缓冲
- [[分支预测]] - 预测失败触发flush
- [[寄存器重命名]] - flush需恢复映射
- [[Load-Store违例]] - RAW触发flush