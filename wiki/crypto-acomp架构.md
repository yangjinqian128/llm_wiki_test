---
title: crypto acomp架构
tags: [linux, crypto, kernel, compression]
created: 2026-05-22
source: raw/notes/crypto/crypto_scomp_acomp_analysis
---

# crypto acomp架构

Linux内核crypto子系统提供同步(scomp)和异步(acomp)压缩接口[^1]。

## API演进

旧API（已弃用）:
```
crypto_register_alg()
crypto_alloc_comp()
```

新API:
```
crypto_register_scomp()  // 同步
crypto_register_acomp()  // 异步
crypto_alloc_acomp()     // 获取实例
```

## scomp特点

- 内部buffer: 每个cpu core 128KB输入/输出buffer[^1]
- 先把scatterlist数据copy到内部buffer
- 调用get_cpu禁止调度（防止同一cpu的并发破坏buffer）[^1]
- compress/decompress函数内部不能做调度[^1]

## acomp特点

- 异步接口，提供回调机制[^1]
- 通过acomp_request_set_callback设置回调[^1]
- 硬件完成后调用回调通知用户

## 使用流程

```c
acomp = crypto_alloc_acomp();
req = acomp_request_alloc(tfm);
acomp_request_set_params(req, &src, &dst, ilen, dlen);
acomp_request_set_callback(req, flags, callback, data);
crypto_acomp_compress(req);
acomp_request_free();
crypto_free_acomp();
```

## 相关概念

- [[Linux-zswap]]
- [[Intel-QAT压缩]]
- [[内核模块]]

[^1]: raw/notes/crypto/crypto_scomp_acomp_analysis