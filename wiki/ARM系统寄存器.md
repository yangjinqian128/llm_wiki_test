---
title: ARM系统寄存器
created: 2026-05-22
tags:
  - ARM
  - 系统寄存器
  - 虚拟化
  - 异常处理
---

# ARM系统寄存器

## 概述

ARM64系统寄存器控制CPU的基础配置、中断异常、地址翻译、时钟等核心功能。

## CPU基础配置寄存器

### 特性描述寄存器

- **ID_AA64DFR[01]_EL1**：debug相关特性
- **ID_AA64ISA[012]_EL1**：ISA指令扩展
- **ID_AA64MMFR[01234]_EL1**：内存模型特性
- **ID_AA64ZFR0_EL1**：SVE特性
- **ID_AA64PFR[012]_EL1**：处理器特性

### 系统信息寄存器

- **MIDR_EL1**：厂商和硬件型号
- **MPIDR_EL1**：多核affinity

### Cache寄存器

- **CTR_EL0**：cache特性（DIC/IDC/L1Ip）
- **CLIDR_EL1**：各级cache类型和属性（LoC/LoUU/LoUIS）

### 浮点/SVE控制

- **CPACR_EL1**：浮点/NEON/SVE使能控制
- **CPTR_EL2/CPTR_EL3**：trap控制

## 中断异常寄存器

- **PSTATE**：CPU状态寄存器
- **SPSR_ELx**：保存的程序状态寄存器
- **VBAR_ELx**：中断异常向量基地址
- **ESR_ELx**：异常原因寄存器
- **FAR_ELx**：异常地址寄存器
- **ELR_ELx**：异常返回地址

## 地址翻译寄存器

- **TTBR0_ELx/TTBR1_ELx**：页表基地址
- **TCR_ELx**：地址翻译控制
- **VTTBR_EL2/VTCR_EL2**：Stage2翻译寄存器（虚拟化）

## Timer寄存器

- **CNTFRQ_EL0**：counter频率
- **CNTPCT_EL0**：物理counter值
- **CNTP_CTL_EL0**：timer控制
- **CNTP_TVAL_EL0**：TimerValue（递减）
- **CNTP_CVAL_EL0**：CompareValue（触发中断）

## 虚拟化寄存器(VHE)

### HCR_EL2配置

- **E2H**：使能VHE
- **TGE**：1=host, 0=guest

### VHE寄存器映射

当VHE使能且运行在EL2时：

```
SCTLR_EL1   → SCTLR_EL2
TTBR0_EL1   → TTBR0_EL2
TTBR1_EL1   → TTBR1_EL2
TCR_EL1     → TCR_EL2
ESR_EL1     → ESR_EL2
...
```

### 别名寄存器

在EL2/EL3访问EL0/EL1寄存器使用XXX_EL12别名，避免VHE映射影响。

## Translation Regime

- **Non-secure EL1&EL0**：虚拟机，需Stage1+Stage2翻译
- **Non-secure EL2&EL0**：host，仅需Stage1翻译

参见[[GIC中断控制器]]、[[ARM原子指令]]。

---

**参考来源**：[ARM64系统寄存器总结](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/ARM64系统寄存器总结)