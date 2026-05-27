---
title: RISC-V特权级模型
created: 2026-05-22
tags:
  - RISC-V
  - 特权级
  - 系统架构
  - CSR寄存器
---

# RISC-V特权级模型

## 概述

RISC-V定义了多个特权级，支持不同级别的软件隔离。主要特权级包括：U(User)、S(Supervisor)、M(Machine)模式。

## 特权级编码

| 模式 | 编码 |
|------|------|
| U(User) | 00 |
| S(Supervisor) | 01 |
| M(Machine) | 11 |

## CSR寄存器访问

CSR(Control System Register)只能通过特殊指令访问：

- **csrrw**：读后写
- **csrrs**：读后置位
- **csrrc**：读后清除

### CSR域段属性

- **WPRI**(Write Preserve Read Ignore)：写不改变，读忽略
- **WLRL**(Write Legal Read Legal)：必须写合法值
- **WARL**(Write Any Read Legal)：可写任意值，读返回合法值

## M模式寄存器

### 机器描述寄存器

- **misa**：ISA扩展支持
- **mvendorid/marchid/mimpid**：厂商信息
- **mhartid**：core编号

### 中断异常寄存器

- **mstatus**：机器状态寄存器
- **mepc**：异常PC
- **mtvec**：中断异常向量基地址
- **mtval**：异常参数
- **medeleg/mideleg**：异常/中断委托
- **mie/mip**：中断使能/pending
- **mcause**：异常原因

### mstatus关键字段

| 字段 | 说明 |
|------|------|
| MIE/SIE | 全局中断使能 |
| MPIE/SPIE | 之前的中断使能状态 |
| MPP/SPP | 之前的特权级 |
| MBE/SBE/UBE | 大小端配置 |
| MPRV | 修改特权级访存 |
| MXR | 使可执行内存可读 |
| SUM | 允许S模式访问U模式内存 |
| TVM | trap虚拟内存操作 |
| TW | trap WFI指令 |
| TSR | trap sret指令 |

## S模式寄存器

- **sstatus**：s模式状态
- **sepc**：异常PC
- **stvec**：中断向量基地址
- **stval**：异常参数
- **sie/sip**：中断使能/pending
- **scause**：异常原因
- **sscratch**：软件使用
- **satp**：页表基地址

## 中断优先级

1. 同级中：External > Software > Timer
2. 高特权级中断优先级更高

## 中断异常流程

1. 硬件保存状态：SIE→SPIE, mode→SPP, SIE清0
2. sepc保存异常/中断PC
3. scause记录原因
4. stval记录参数
5. PC跳转到stvec
6. sret返回：SPIE→SIE, SPP→mode

## 特权级指令

- **ecall**：系统调用
- **ebreak**：断点
- **sret/mret**：从S/M模式返回
- **wfi**：等待中断（低功耗）
- **sfence.vma**：TLB刷新

参见[[RISC-V中断处理]]、[[RISC-V调用约定]]。

---

**参考来源**：[rv特权级模型](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/riscv/rv特权级模型)