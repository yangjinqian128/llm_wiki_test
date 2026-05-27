---
type: knowledge-node
tags: [core-concept, auto-generated, debug, kernel]
last_compiled: 2026-05-22
---

# kexec

[[kexec]]从运行中的内核直接加载新内核，绕过BIOS[^1]。

## 启动时间对比

| 方式 | 时间 |
|------|------|
| 普通启动 | BIOS → GRUB → 内核（约60秒）[^1] |
| kexec启动 | 当前内核 → kexec → 新内核（约5秒）[^1] |

## 基本操作

1. 安装新内核RPM[^1]。

2. 加载内核[^1]：
```bash
sudo kexec -l /boot/vmlinuz-新版本 \
    --initrd=/boot/initramfs-新版本.img \
    --append="root=/dev/xxx ro"
```

使用`--reuse-cmdline`复用当前启动参数[^1]。

3. 执行快速重启[^1]：
```bash
sudo kexec -e
```

## 生成initramfs

```bash
dracut --force initramfs-新版本.img 新版本
```
[^1]

## 适用场景

- 内核开发调试[^1]
- 驱动调试[^1]
- 快速验证补丁[^1]

## 相关概念

- [[内核调试]] - 调试技术
- [[内核启动]] - 启动流程
- [[initramfs]] - 初始化内存文件系统

[^1]: 来源：raw/notes/debug/kexec_快速调试内核.md