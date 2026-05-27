---
title: ARM向量指令
created: 2026-05-22
tags:
  - ARM
  - SIMD
  - 向量化
  - SVE
  - NEON
---

# ARM向量指令

## 概述

ARM向量指令用于处理具有并行度的数据，通过特殊的向量指令和寄存器提升性能。

## 发展历程

- **NEON**：ARMv7引入的向量指令集
- **SVE**(Scalable Vector Extension)：ARMv8引入，支持可扩展向量长度
- **SVE2**：ARMv9引入
- **SME**(Scalable Matrix Extension)：ARMv9引入，面向矩阵运算

## 寄存器资源

### 向量寄存器

- **Z0-Z31**：32个向量寄存器，最大长度2048bit
- **ZCR_ELx**：控制向量长度

### 谓词寄存器

- **P0-P15**：16个谓词寄存器
- P0-P7：访存和计算谓词
- P8-P15：循环控制谓词

### FFR(First Fault Register)

记录向量访存指令的异常信息。

## SVE指令示例

### ADD指令编码

```
+-----------------+-------+-----------------+----+--------+-------+
| 31 - 24         | 23 22 | 21 - 14         | 13 | 12 - 5 | 4 - 0 |
+-----------------+-------+-----------------+----+--------+-------+
| 0 0 1 0 0 1 0 1 | size  | 1 0 0 0 0 0 1 1 | sh | imm8   | Zdn   |
+-----------------+-------+-----------------+----+--------+-------+
```

size字段编码元素大小：8/16/32/64bit

### 带谓词的ADD

```
ADD Zdn.T Pg/M, Zdn.T, Zm.T
```

根据Pg寄存器中的bit mask对有效位做ADD操作，M(merge)表示无效位不做处理。

## 内存访问

### Gather-load/Scatter-store

使用向量寄存器表示一组地址，实现批量访存操作。

### 异常处理版本

- **LD1B**：每次访存可能产生异常
- **LDFF1B**(first fault)：只看第一个有效数据的异常
- **LDNF1B**(no fault)：不产生异常，异常信息记录到FFR

## 上下文管理

Linux内核采用lazy策略保存恢复向量寄存器上下文（`arch/arm64/kernel/fpsimd.c`）。

## ABI考虑

需要定义包含向量寄存器的过程调用协议，参见[[ARM-ABI|调用约定]]。

## 性能考量

主要用户是HPC等特定场景。Apple M系列处理器不支持SVE，Intel AVX曾被认为"不务正业"。

---

**参考来源**：[arm64_vector](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/arm64_vector)