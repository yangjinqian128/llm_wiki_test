---
title: KAE加速器
tags: [crypto, openssl, hardware, kunpeng]
created: 2026-05-22
source: raw/notes/crypto/KAE_note
---

# KAE加速器

KAE是华为KunPeng服务器上加速器模块对应的openssl engine实现[^1]。

## 架构概述

KAE作为OpenSSL engine注册到系统中，使用wd队列模型处理加解密任务[^1]。

### 注册流程

```
IMPLEMENT_DYNAMIC_BIND_FN(bind_kae)
  +-> kae_engine_setup()
      +-> cipher_module_init()      // 初始化sec引擎
          +-> wd_ciphers_init_qnode_pool()
          +-> sec_create_ciphers()
              +-> sec_ciphers_set_cipher_method()
      +-> async_module_init()       // 异步轮询线程
  +-> ENGINE_set_ciphers(e, sec_engine_ciphers)
```

## 队列模型

- 每次上层用户请求创建一个实例，每个实例里申请一个wd队列
- 反复向这一个队列上发送任务，复用队列和ctx[^1]
- 异步任务发送后，把队列信息加入软件队列，异步轮询线程处理

## 性能考量

1. 最大并发请求个数约2048
2. 同步请求会有大的并发（存储场景spark/ceph）
3. 异步请求并发不大，少量线程可跑满硬件性能
4. 一个engine一个异步轮询队列满足性能需求[^1]

## 相关概念

- [[对称加密]]
- [[OpenSSL性能测试]]
- [[硬件加速器]]

[^1]: raw/notes/crypto/KAE_note