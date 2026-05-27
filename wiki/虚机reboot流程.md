---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization, KVM, QEMU]
last_compiled: 2026-05-22
---

# 虚机reboot流程

[[虚机reboot流程]] 描述 Linux KVM 环境下 guest 虚拟机重启的完整流程，涉及 Guest Linux、Host KVM 和 [[QEMU]] 三层协作[^1]。

## Guest Linux reboot

Guest Linux reboot 流程与 host 一致[^1]：

```
reboot
  → kernel_restart
    → machine_restart              // arch/arm64/kernel/process.c
      → smp_send_stop              // 发IPI停止其它core
      → efi_reboot                 // 调用EFI runtime service
```

EFI runtime service 通过 [[PSCI]] 的 `PSCI_1_1_FN64_SYSTEM_RESET2` 实现[^1]。

## Host KVM 处理

[[PSCI]] 使用 smc/hvc 实现，虚拟机调用会 trap 到 [[KVM]][^1]。

KVM 模拟实现调用流程：

```
kvm_arch_vcpu_ioctl_run
  → handle_exit
    → handle_trap_exceptions
      → handle_smc/handle_hvc
        → kvm_smccc_call_handler
          → kvm_psci_call
            → kvm_psci_1_x_call
              → kvm_psci_system_reset2
                → kvm_prepare_system_event
```

`kvm_prepare_system_event` 设置退出原因和类型[^1]：

- `vcpu->run->exit_reason = KVM_EXIT_SYSTEM_EVENT`
- `vcpu->run->system_event.type = KVM_SYSTEM_EVENT_RESET`

然后从 `ioctl_run` 返回到 [[QEMU]][^1]。

## QEMU 处理

### vCPU 线程

[[QEMU]] vCPU 线程主线逻辑：

```
kvm_vcpu_thread_fn
  kvm_init_vcpu
  do {
    if (cpu_can_run(cpu)) {
      kvm_cpu_exec(cpu)
        kvm_vcpu_ioctl           // 运行vCPU
        switch (run->exit_reason)
          case KVM_EXIT_SYSTEM_EVENT
            qemu_system_reset_request
              cpu_stop_current   // stop当前vCPU
              qemu_notify_event   // 触发主线程
    }
    qemu_wait_io_event(cpu)
  } while (!cpu->unplug || cpu_can_run(cpu))
```

### QEMU 主线程

主线程处理 reset 请求：

```
main
  → qemu_init
  → qemu_main_loop
    → while (!main_loop_should_exit(&status)) {
        main_loop_wait(false)
      }

main_loop_should_exit
  → qemu_reset_requested
    → pause_all_vcpus
    → qemu_system_reset
      → cpu_synchronize_all_states
          → cpu_synchronize_state
              → kvm_cpu_synchronize_state  // KVM寄存器读入QEMU
      → qemu_devices_reset / mc->reset
          → resettable_reset
      → cpu_synchronize_all_post_reset
          → cpu_synchronize_post_reset
              → kvm_cpu_synchronize_post_reset  // QEMU寄存器写回KVM
    → resume_all_vcpus
```

## 设备 Reset

`qemu_devices_reset` 触发各设备 reset 回调：

### CPU Reset

```
arm_cpu_reset_hold
  → kvm_arm_reset_vcpu
    → kvm_arm_vcpu_init         // KVM_ARM_VCPU_INIT ioctl
    → write_kvmstate_to_list
    → write_list_to_cpustate
```

### GICv3 Reset

```
kvm_arm_gicv3_reset_hold
  → parent_phases.hold          // arm_gicv3_common_reset_hold
  → kvm_arm_gicv3_put           // 配置KVM GICv3状态
```

热迁移恢复 [[GICv3]] 状态时也使用此接口[^1]。

### ITS Reset

```
kvm_arm_its_reset_hold
  → parent_phases.hold          // gicv3_its_common_reset_hold
  → KVM_DEV_ARM_ITS_CTRL_RESET ioctl + ITS寄存器刷新
```

## Reset 回调注册

各设备 reset_hold 回调注册位置：

**CPU** - `target/arm/cpu.c`[^1]：
```c
arm_cpu_class_init
  → resettable_class_set_parent_phases(rc, NULL, arm_cpu_reset_hold, NULL,
                                       &acc->parent_phases)
```

**GICv3** - `hw/intc/arm_gicv3_kvm.c`[^1]：
```c
kvm_arm_gicv3_class_init
  → resettable_class_set_parent_phases(rc, NULL, kvm_arm_gicv3_reset_hold, NULL,
                                       &kgc->parent_phases)
```

**ITS** - `hw/intc/arm_gicv3_its_kvm.c`[^1]：
```c
kvm_arm_its_class_init
  → resettable_class_set_parent_phases(rc, NULL, kvm_arm_its_reset_hold, NULL,
                                       &ic->parent_phases)
```

## 相关概念

- [[PSCI]] - ARM 电源管理接口
- [[KVM]] - 内核虚拟机
- [[QEMU]] - 虚拟机监控器
- [[GICv3]] - ARM 中断控制器
- [[ITS]] - 中断翻译服务
- [[热迁移]] - 虚拟机热迁移（使用相似的设备状态同步机制）

---
[^1]: 来源：raw/notes/virt/基于QEMU的Linux_reboot流程分析