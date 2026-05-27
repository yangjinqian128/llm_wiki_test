---
title: ACLINT中断控制器
created: 2026-05-22
tags:
  - RISC-V
  - ACLINT
  - 中断
  - 时钟
---

# ACLINT中断控制器

## 概述

ACLINT(Advanced Core Local Interrupt)是RISC-V的核内本地中断控制器，提供时钟中断和核间中断功能。

## 与CLINT的关系

- **CLINT**：原始本地中断控制器
- **ACLINT**：高级版本，兼容CLINT

所有功能通过MMIO寄存器控制。

## 设备类型

| 设备 | 说明 |
|------|------|
| Machine-level Timer Device (mtimer) | 时钟中断 |
| Machine-level Software Interrupt Device (mswi) | M模式核间中断 |
| Supervisor-level Software Interrupt Device (sswi) | S模式核间中断 |

## 软件中断

### CLINT方式

只有msip寄存器，最低bit映射到CSR寄存器MIP的MSIP bit，触发其他核的M mode soft interrupt。

### ACLINT方式

兼容msip寄存器，新增SETSSIP寄存器。

- **mswi**：封装msip寄存器
- **sswi**：封装SETSSIP寄存器

## 寄存器行为

| 操作 | mswi | sswi |
|------|------|------|
| 读 | 返回MIP_MSIP值 | 返回0 |
| 写1 | 触发高电平中断 | 触发高电平中断 |
| 写0 | 拉低中断线 | 无动作 |

## QEMU支持

启动参数：`-machine virt,aclint=on`

设备树节点变化：
- 未启用：单个`riscv,clint0`节点
- 启用后：mtimer/sswi/mswi三个节点

参见[[PLIC中断控制器]]、[[RISC-V特权级模型]]。

---

**参考来源**：[riscv_aclint](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/riscv/riscv_aclint)