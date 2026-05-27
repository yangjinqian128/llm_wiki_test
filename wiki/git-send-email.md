---
title: git send-email
tags: [git, email, patch]
created: 2026-05-22
source: raw/notes/git/git_doc_3
---

# git send-email

发送patch系列邮件[^1]。

## 基本用法

```bash
git send-email *.patch
git send-email *.patch --cc=email@example.com
```

- 提供对话式发送过程[^1]
- patch组成系列，保持关联[^1]
- 比普通邮箱发送保持patch完整性[^1]

## Message-ID回复

可以选择Message-ID作为In-Reply-To[^1]。

## 相关概念

- [[git patch制作]]
- [[GitHub PR流程]]
- [[Git基础操作]]

[^1]: raw/notes/git/git_doc_3