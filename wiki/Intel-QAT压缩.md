---
title: Intel QAT压缩
tags: [crypto, compression, intel, hardware]
created: 2026-05-22
source: raw/notes/crypto/qat_zip
---

# Intel QAT压缩

Intel QAT(QuickAssist Technology)提供硬件压缩解压缩引擎[^1]。

## 软件栈架构

```
用户APP (如ceph)
    ↓
QATzip库 (github.com/intel/QATzip)
    ↓
libqat用户态基础库 (01.org)
    ↓
内核QAT驱动 + UIO子系统
    ↓
QAT硬件
```

## 关键点

1. 内核驱动通过UIO暴露硬件资源给用户态（安全问题导致无法上传主线）[^1]
2. libqat封装UIO接口提供基础API[^1]
3. QATzip调用libqat提供压缩接口[^1]
4. ceph可通过HAVE_QATZIP宏使用硬件压缩[^1]

## 相关概念

- [[crypto acomp架构]]
- [[硬件加速器]]
- [[zswap]]

[^1]: raw/notes/crypto/qat_zip