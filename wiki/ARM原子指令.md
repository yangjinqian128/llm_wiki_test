---
title: ARM原子指令
created: 2026-05-22
tags:
  - ARM
  - 原子操作
  - 并发
  - 指令集
---

# ARM原子指令

## 概述

ARM架构提供了多种原子指令用于并发编程，主要包括Load-Exclusive/Store-Exclusive指令集和LSE(Large System Extensions)原子指令。

## LDXR/STXR指令

ARM采用load-reserved/store-conditional方式实现原子操作：

- **LDXR/STXR**：基本原子操作指令
- **LDAXR/STLXR**：带load-acquire/store-release属性的版本
- 支持不同位宽：XR(寄存器)、XRB(8bit)、XRH(16bit)、XP(双寄存器)

与[[RISC-V特权级模型|RISC-V]]的LR/SC指令逻辑相似。

### LDXR与WFE的巧用

LDXR会设置exclusive标记，当其他store操作清除该标记时，系统会触发event register的set操作，可用于条件读：

```c
/* linux/arch/arm64/include/asm/cmpxchg.h */
#define __CMPWAIT_CASE(w, sfx, sz)                    \
static inline void __cmpwait_case_##sz(volatile void *ptr,    \
                                      unsigned long val)    \
{                                \
    unsigned long tmp;                    \
                                \
    asm volatile(                        \
    "   sevl\n"                        \
    "   wfe\n"                        \
    "   ldxr" #sfx "\t%" #w "[tmp], %[v]\n"        \
    "   eor %" #w "[tmp], %" #w "[tmp], %" #w "[val]\n"    \
    "   cbnz %" #w "[tmp], 1f\n"                \
    "   wfe\n"                        \
    "1:"                            \
    : [tmp] "=&r" (tmp), [v] "+Q" (*(u##sz *)ptr)        \
    : [val] "r" (val));                    \
}
```

## LSE原子指令

ARMv8.1 LSE新增单指令原子操作：

### 1. 原子运算指令

命名规则：LD/ST + 运算种类
- **LDADD**：原子加，返回旧值
- **STADD**：原子加，无返回值

示例：`LDADD x1, x2, [x3]` - 将x2和[x3]相加存入[x3]，旧值存入x1

### 2. 交换指令

- **SWP/SWPB/SWPH**：原子交换内存与寄存器的值

### 3. CAS指令

- **CAS/CASB/CASH/CASP**：compare and swap指令
- CASP支持128bit原子操作

## Single Copy 64B指令

ARMv8.7引入LD64B/ST64B等指令，用于原子操作MMIO空间64字节数据。

## WFI/WFE/SEV指令

- **WFI**(Wait For Interrupt)：进入低功耗模式，等待中断唤醒
- **WFE**(Wait For Event)：睡眠等待event发生
- **SEV**(Send Event)：唤醒所有core上的WFE
- **SEVL**(Send Event Local)：set当前PE的event register

## 预取指令

- **PRFM**：数据预取指令，可提前加载数据到cache

---

**参考来源**：[ARM构架下原子操作相关指令总结](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ARM/ARM构架下原子操作相关指令总结)