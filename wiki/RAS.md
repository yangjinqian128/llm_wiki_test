---
title: RAS
aliases: [Reliability Availability Serviceability]
tags: [hardware, error, arm64]
source: raw/notes/RAS/ARM64_Firmware_First_SEA_SEI_RAS_Handling.md
---

# RAS

ARMv8.2-A引入的可靠性、可用性、可服务性扩展，提供硬件错误检测和报告机制。

## 异常类型

| 类型 | 说明 | 返回地址 |
|------|------|----------|
| SEA | 同步外部中止 | 触发异常的指令 |
| SEI | 异步外部中止 | 下一条指令 |

## 异常路由

通过`SCR_EL3`配置：
- `EA=1`：External Abort路由到EL3
- `EA=0`：根据异常级别路由

## Firmware First模式

```
SEA/SEI发生
  ↓
硬件路由到EL3（固件）
  ↓
固件处理，查看私有硬件错误信息
  ↓
将信息写入APEI表
  ↓
返回OS异常向量入口
  ↓
OS读取APEI表获取详情
```

## Linux内核处理

```c
do_sea()
  → arm64_notify_die()
    → 用户态：arm64_force_sig_fault（终止进程）
    → 内核态：die() → crash_kexec() → panic()
```

## 相关概念

- [[APEI]] - ACPI Platform Error Interface
- [[固件优先]] - Firmware First处理模式
- [[SEA]] - 同步外部中止
- [[内核调试]] - crash分析