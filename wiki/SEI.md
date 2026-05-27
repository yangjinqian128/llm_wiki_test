---
type: knowledge-node
tags: [core-concept, auto-generated, ras]
last_compiled: 2026-05-22
---

# SEI

[[SEI]]（SError Interrupt）是异步外部中止[^1]。

## 特点

与当前指令执行无直接关联[^1]。

异常返回地址指向下一条指令[^1]。

也称为SError[^1]。

## 路由配置

`SCR_EL3.EA`控制路由[^1]：
- EA=1：External Abort路由到EL3[^1]
- EA=0：路由基于异常级别[^1]

## 与SEA区别

**SEA**：同步，当前指令触发[^1]。

**SEI**：异步，与指令无关[^1]。

## 处理流程

硬件 → 固件(EL3) → [[APEI]] → OS[^1]。

## 双重错误

一个RAS错误源可能触发SEA和SEI[^1]。

SEA处理未关中断时SEI被taken[^1]。

SEI看到SEA栈信息[^1]。

## 相关概念

- [[SEA]] - 同步外部中止
- [[RAS]] - RAS扩展
- [[固件优先]] - 固件优先模式

[^1]: 来源：raw/notes/RAS/ARM64_Firmware_First_SEA_SEI_RAS_Handling.md