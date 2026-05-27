---
type: knowledge-node
tags: [core-concept, auto-generated, device-driver]
last_compiled: 2026-05-22
---

# mmap设备

[[mmap设备]]将设备IO空间映射到用户态[^1]。

## 基本用途

用户态像访问内存一样访问设备[^1]。

直接访问设备寄存器[^1]。

## 驱动实现

实现file_operations的mmap回调[^1]。

## vm_area_struct

用户态想要映射的虚拟地址[^1]。

传递给驱动[^1]。

## remap_pfn_range

建立虚拟地址到物理地址映射[^1]。

参数[^1]：
- vma[^1]
- start：虚拟地址[^1]
- pfn：物理页帧号[^1]
- size[^1]
- prot：页保护[^1]

## 示例实现

```
int scull_mmap(struct file *file, struct vm_area_struct *vma)
{
    unsigned long page = virt_to_phys(scull_device->mmap_memory);
    unsigned long start = (unsigned long)vma->vm_start;
    unsigned long size = (unsigned long)(vma->vm_end - vma->vm_start);
    vma->vm_flags |= (VM_IO | VM_LOCKED | VM_DONTEXPAND | VM_DONTDUMP);

    if (remap_pfn_range(vma, start, page >> PAGE_SHIFT, size, PAGE_SHARED))
        return -1;

    return 0;
}
```
[^1]

## 内核内存映射

get_free_page分配的内存也可映射[^1]。

并非只能映射IO空间[^1]。

## 进程地址空间

`cat /proc/pid/maps`查看[^1]。

显示映射区域[^1]。

## 相关概念

- [[vm_area_struct]] - 虚拟内存区域
- [[字符设备]] - 字符设备驱动

[^1]: 来源：raw/notes/LDD/scull_3