---
title: Linux crypto子系统
tags: [linux, crypto, kernel, driver]
created: 2026-05-22
source: raw/notes/crypto/crypto_note
---

# Linux crypto子系统

Linux内核crypto子系统提供加解密、压缩解压缩等功能[^1]。

## 架构

```
crypto API <---> crypto core <---> crypto_register_alg
```

设备驱动通过crypto_register_alg注册算法到crypto系统[^1]。

## 关键数据结构

### crypto_alg

```c
struct crypto_alg {
    u32 cra_flags;
    unsigned int cra_blocksize;
    unsigned int cra_ctxsize;     // 上下文大小
    char cra_name[CRYPTO_MAX_ALG_NAME];
    union {
        struct ablkcipher_alg ablkcipher;
        struct compress_alg compress;
    } cra_u;
    int (*cra_init)(struct crypto_tfm *tfm);
    void (*cra_exit)(struct crypto_tfm *tfm);
};
```

### crypto_tfm

```c
struct crypto_tfm {
    u32 crt_flags;
    union {
        struct compress_tfm compress;
    } crt_u;
    void (*exit)(struct crypto_tfm *tfm);
    struct crypto_alg *__crt_alg;
    void *__crt_ctx[];           // 私有上下文
};
```

## 同步接口使用

```c
// 1. 分配上下文
comp = crypto_alloc_comp(driver, type, mask);

// 2. 执行压缩
crypto_comp_compress(comp, src, slen, dst, dlen);

// 3. 释放
crypto_free_comp(comp);
```

## 异步接口使用

```c
acomp = crypto_alloc_acomp();
req = acomp_request_alloc(tfm);
acomp_request_set_params(req, &src, &dst, ilen, dlen);
acomp_request_set_callback(req, flags, callback, wait);
crypto_acomp_compress(req);
```

## 相关概念

- [[crypto acomp架构]]
- [[内核模块]]
- [[设备驱动]]

[^1]: raw/notes/crypto/crypto_note