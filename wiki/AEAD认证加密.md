---
title: AEAD认证加密
tags: [crypto, authentication, encryption]
created: 2026-05-22
source: raw/notes/crypto/aead
---

# AEAD认证加密

AEAD（Authenticated Encryption with Associated Data）是同时提供加密和认证的密码模式[^1]。

## 问题背景

对于加解密的内容，传送中途可能被人更改，需要同时保证机密性和完整性[^1]。

## 常见模式

- **GCM**: 依赖CTR模式，是最常用的AEAD模式[^1]
- **CCM**: 基于CBC的消息认证码[^1]
- **HMAC**: 使用hash生成消息认证码（带密钥的hash）[^1]

## 相关概念

- [[对称加密]]
- [[分组密码模式]]
- [[哈希函数]]
- [[数字签名]]

[^1]: raw/notes/crypto/aead