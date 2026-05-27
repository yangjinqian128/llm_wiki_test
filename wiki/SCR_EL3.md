---
type: knowledge-node
tags: [core-concept, auto-generated, ras, arm]
last_compiled: 2026-05-22
---

# SCR_EL3

[[SCR_EL3]]是安全配置寄存器[^1]。

## 功能

控制异常路由到EL3[^1]。

## 关键位段

### EA（Bit 3）

External Abort路由[^1]：
- 1：External Abort/SError路由到EL3[^1]
- 0：路由基于异常级别[^1]

### FIQ（Bit 2）

快速中断路由[^1]：
- 1：FIQ路由到EL3[^1]

### IRQ（Bit 1）

中断请求路由[^1]：
- 1：IRQ路由到EL3[^1]

## 固件优先配置

配置`SCR_EL3.EA=1`[^1]。

[[SEA]]/[[SEI]]路由到固件[^1]。

固件查看私有错误信息[^1]。

## 相关概念

- [[SEA]] - 同步外部中止
- [[SEI]] - 异步外部中止
- [[固件优先]] - 固件优先模式
- [[RAS]] - RAS扩展

[^1]: 来源：raw/notes/RAS/ARM64_Firmware_First_SEA_SEI_RAS_Handling.md