---
type: knowledge-node
tags: [core-concept, auto-generated, KVM, ARM64, FPSIMD]
last_compiled: 2026-05-22
---

# FPMR (Floating Point Mode Register)

FPMR 是 ARMv9.4 引入的浮点模式寄存器，用于控制 FP8（8-bit 浮点）运算[^1]。

## 特性

- 不区分 EL1/EL2，单一物理寄存器[^1]
- 编码 `S3_3_C4_C4_2`[^1]

## KVM Lazy Switch

FPMR 跟随 [[FPSIMD]] 状态一起做 lazy switch[^1]：

- guest 不用 FP 指令不切 FPMR[^1]
- guest 用 FP（CPTR_EL2 陷阱）才在 trap handler 中切[^1]

## 保存/恢复流程

**Host 保存** (陷阱处理中)[^1]：

```c
if (fp_owner == FP_STATE_HOST_OWNED) {
    __fpsimd_save_state(...);
    if (system_supports_fpmr())
        *host_data_ptr(fpmr) = read_sysreg_s(SYS_FPMR);
}
```

**Guest 恢复**[^1]：

```c
if (kvm_has_fpmr(vcpu->kvm))
    write_sysreg_s(__vcpu_sys_reg(vcpu, FPMR), SYS_FPMR);
```

**惰性回写** (VM exit)[^1]：

- 注册指针 `fp_state.fpmr = &__vcpu_sys_reg(vcpu, FPMR)`
- host 需要 FP 硬件时通过指针自动回写

## 相关概念

- [[KVM]]
- [[FPSIMD]]
- [[ARMv9]]

[^1]: 来源: raw/notes/ai_gen/KVM_FPMR_Save_Restore_Patch_Analysis.md