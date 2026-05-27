---
title: OpenSSL性能测试
tags: [crypto, openssl, benchmark]
created: 2026-05-22
source: raw/notes/crypto/openssl
---

# OpenSSL性能测试

OpenSSL提供多种性能测试命令和方法[^1]。

## 基本命令

### AES对称加解密

```bash
openssl enc -aes-128-cbc -in data -out encrypt -K key -iv iv
openssl enc -aes-128-cbc -in encrypt -out decrypt -K key -iv iv -d
```

### RSA非对称加解密

```bash
# 密钥生成
openssl genrsa -out test.key -engine uadk 4096
openssl rsa -in test.key -pubout -out test_pub.key -engine uadk

# 加解密
openssl rsautl -encrypt -in file -inkey test_pub.key -pubin -out encrypted
openssl rsautl -decrypt -in encrypted -inkey test.key -out decrypted

# 签名认证
openssl rsautl -sign -in msg.txt -inkey test.key -out signed.txt
openssl rsautl -verify -in signed.txt -inkey test_pub.key -pubin
```

### 哈希

```bash
openssl md5/sha1/sha256/sm3 -engine uadk data
```

## speed命令

```bash
openssl speed aes
openssl speed rsa
openssl speed -engine uadk -async_jobs 1 -evp md5
openssl speed -engine uadk -elapsed rsa2048
```

- `-engine`: 使用硬件引擎
- `-async_jobs`: 使用异步机制
- `-elapsed`: 测试实际耗时[^1]

## 相关概念

- [[对称加密]]
- [[非对称加密]]
- [[KAE加速器]]
- [[哈希函数]]

[^1]: raw/notes/crypto/openssl