---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, arm]
last_compiled: 2026-05-22
---

# SMCCC

[[SMCCC]]（SMC Call Convention）是ARM定义的SMC/HVC访问标准[^1]。

## 基本机制

- **SMC**：trap到EL3[^1]
- **HVC**：trap到EL2[^1]

使用寄存器传递参数和返回值[^1]。

## 接口类别

接口种类编码到w0寄存器[^1]：
1. Arm Architecture Calls[^1]
2. CPU Service Calls[^1]
3. SiP Service Calls[^1]
4. OEM Service Calls[^1]
5. Standard Secure Service Calls[^1]
6. Standard Hypervisor Service Calls[^1]
7. Vendor Specific Hypervisor Calls[^1]

## PSCI保留

[[PSCI]]、[[SDEI]]在Standard Secure Service Calls保留空间[^1]。

## smc/hvc选择

由`psci_conduit`决定[^1]。

配置来源：ACPI FADT表`boot_flags`[^1]。

QEMU配置逻辑[^1]：
- guest有EL2：使用smc[^1]
- guest只有EL0/EL1：使用hvc[^1]

## KVM模拟

```
handle_smc/handle_hvc -> kvm_smccc_call_handler
```
[^1]

## SMCCC Filter

KVM提供ioctl配置function处理方式[^1]：
- 处理[^1]
- 不处理[^1]
- 到用户态处理[^1]

## Firmware Register

每种接口类型对应一个寄存器[^1]。

每个接口占1个bit[^1]。

迁移时需考虑[^1]。

## PV特性通信

[[半虚拟化]]特性通过SMCCC通信[^1]。

如[[pvtime]]的function id定义在Standard Hypervisor Service Call[^1]。

## 相关概念

- [[PSCI]] - 电源状态协调接口
- [[半虚拟化]] - PV特性
- [[KVM]] - 内核虚拟化

[^1]: 来源：raw/notes/virt/ARM_SMCCC基本逻辑