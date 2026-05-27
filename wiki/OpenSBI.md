---
title: OpenSBI
aliases: [RISC-V SBI, opensbi]
tags: [riscv, firmware, sbi]
source: raw/notes/firmware/opensbi_note
---

# OpenSBI

RISC-V SBI（Supervisor Binary Interface）标准的实现，为特权态软件提供与机器的二进制接口。

## SBI标准

https://github.com/riscv-non-isa/riscv-sbi-doc

## 代码组织

- `libsbi`：SBI公共代码库
- `libsbiutils`：SBI工具库
- `platform/generic`：QEMU virt平台实现

## 固件类型

| 类型 | 说明 |
|------|------|
| fw_jump.elf | 跳到指定地址运行下一阶段 |
| fw_load.elf | 打包内核到固件 |
| fw_dynamic.elf | 定义上下阶段接口 |

## 启动流程

1. fw_base.S执行
2. 选择boot hart（lottery或固定）
3. relocate（加载地址≠链接地址时）
4. 清BSS段
5. 初始化scratch空间
6. 配置trap handler
7. 调用sbi_init

## 多核启动

- 主核执行公共初始化
- 从核等待_boot_status变化
- 从核从_start_warm继续执行

## 相关概念

- [[RISC-V特权级模型]] - M/S/U模式
- [[RISC-V中断处理]] - trap机制
- [[多核启动]] - SMP启动
- [[固件]] - 启动固件概念