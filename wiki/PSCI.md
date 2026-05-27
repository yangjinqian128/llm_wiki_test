---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, arm]
last_compiled: 2026-05-22
---

# PSCI

[[PSCI]]（Power State Coordination Interface）是ARM电源管理接口[^1]。

## 实现方式

通过[[SMCCC]]使用smc/hvc实现[^1]。

## Reboot流程

guest调用`PSCI_1_1_FN64_SYSTEM_RESET2`[^1]。

trap到[[KVM]]模拟[^1]。

## KVM处理

```
kvm_psci_call -> kvm_psci_system_reset2
```

设置`vcpu->run->exit_reason = KVM_EXIT_SYSTEM_EVENT`[^1]。

## QEMU处理

```
kvm_cpu_exec -> KVM_EXIT_SYSTEM_EVENT -> qemu_system_reset_request
```

stop当前vCPU，触发主线程[^1]。

## Reset流程

1. cpu_synchronize_all_states[^1]
2. qemu_devices_reset[^1]
   - arm_cpu_reset_hold[^1]
   - kvm_arm_gicv3_reset_hold[^1]
   - kvm_arm_its_reset_hold[^1]
3. cpu_synchronize_all_post_reset[^1]
4. resume_all_vcpus[^1]

## conduit选择

由ACPI FADT决定[^1]。

## 相关概念

- [[SMCCC]] - SMC调用约定
- [[KVM]] - 内核虚拟化
- [[虚拟化]] - 虚拟化技术

[^1]: 来源：raw/notes/virt/基于QEMU的Linux_reboot流程分析
[^2]: 来源：raw/notes/virt/ARM_SMCCC基本逻辑