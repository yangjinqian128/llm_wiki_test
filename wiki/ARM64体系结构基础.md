---
title: ARM64体系结构基础
created: 2026-05-22
tags:
  - ARM
  - ARM64
  - 体系结构
---

# ARM64体系结构基础

## 概述

ARM64(ARMv8/v9)是ARM的64位架构，引入了新的指令集和特权级模型。

## 发展历程

- 2012年推出64位A53和A57核心
- ARMv8定义了64位架构
- ARMv9进一步扩展特性

## 32/64位兼容

ARMv8兼容ARMv7 32位指令的方式是通过模式切换：

- v7和v8使用不同的指令编码空间
- CPU提供模式切换开关
- 控制CPU运行32位或64位指令集

## 特权级模型

ARMv8定义四个特权级：

| 特权级 | 用途 |
|--------|------|
| EL0 | 用户态 |
| EL1 | 内核态 |
| EL2 | Hypervisor |
| EL3 | Secure Monitor |

## 寄存器资源

### 通用寄存器

- 31个64bit通用寄存器(X0-X30)
- X31为零寄存器

### 系统寄存器

- **PSTATE**：CPU当前状态
- **SP_ELx**：各特权级栈指针
- **SPSR_ELx**：异常时保存PSTATE
- **ELR_ELx**：异常返回地址
- **PC**：程序计数器（独立寄存器）

参见[[ARM系统寄存器]]、[[ARM原子指令]]、[[GIC中断控制器]]。

---

**参考来源**：[arm64体系结构编程与实践读书笔记](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/arm64体系结构编程与实践读书笔记)