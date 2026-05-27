---
title: GPG加密工具
tags: [crypto, gpg, encryption, tool]
created: 2026-05-22
source: raw/notes/crypto/gpg
---

# GPG加密工具

GPG(GNU Privacy Guard)是Linux下的加密工具[^1]。

## 对称加密

```bash
# 加密
gpg -c test_file
# 弹出对话框输入密码，生成test_file.gpg (AES192加密)

# 解密
gpg -o test_file -d test_file.gpg
```

同一Linux主机账户下解密不需要密码，另一台机器解密需要输入密码[^1]。

## 相关概念

- [[对称加密]]
- [[哈希函数]]
- [[数字签名]]

[^1]: raw/notes/crypto/gpg