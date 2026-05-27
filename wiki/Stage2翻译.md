---
type: knowledge-node
tags: [core-concept, auto-generated, virtualization]
last_compiled: 2026-05-22
---

# Stage2翻译

[[Stage2翻译]]是虚拟化两级地址翻译的第二级[^1]。

## 地址翻译

- **Stage1**：Guest VA → Guest PA（IPA）[^1]
- **Stage2**：IPA → Host PA[^1]

## 页表基地址

RISC-V：`hgatp`寄存器[^1]。

## 缺页处理

第二级翻译缺页[^1]：

```
gstage_page_fault -> kvm_riscv_gstage_map
```

分配内存并创建第二级页表[^1]。

映射：gpa → pa[^1]。

## MMIO触发

未对MMIO gpa做第二级映射[^1]。

触发缺页异常[^1]。

KVM处理后退出虚拟机[^1]。

## GPA配置

虚拟机物理地址是host用户态分配的虚拟内存[^1]。

三个地址对应相同实际内存[^1]：
1. 虚拟地址VA[^1]
2. 物理地址PA[^1]
3. 虚拟机物理地址GPA[^1]

## 相关概念

- [[虚拟化]] - 虚拟化技术
- [[MMU]] - 内存管理单元
- [[TLB]] - 地址转换缓存
- [[KVM]] - 内核虚拟化

[^1]: 来源：raw/notes/virt/linux_kvm_note