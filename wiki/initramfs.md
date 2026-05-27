---
title: initramfs
aliases: [初始内存文件系统]
tags: [boot, filesystem, init]
---

# initramfs

内核启动后挂载的初始内存文件系统，包含启动必要模块和脚本。

## 生成工具

- dracut - Fedora/RHEL系
- mkinitramfs - Debian系

## 内容

- 必要内核模块
- 启动脚本
- 设备工具（udev等）

## kexec场景

参见[[kexec]]，可手动生成initramfs：
```bash
dracut --force initramfs-版本.img 版本
```

## 相关概念

- [[模块自动加载]] - initramfs中模块加载
- [[udevd]] - 用户态设备管理
- [[kexec]] - 快速内核加载
- [[内核启动]] - 启动流程