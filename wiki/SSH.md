---
title: SSH
aliases: [ssh, Secure Shell]
tags: [网络, ssh, 远程连接, 安全]
source: raw/notes/ssh_note
---

# SSH

Secure Shell，安全的远程登录协议和工具。

## 基本用法

```bash
ssh user_name@remote_host
```

`remote_host` 可以是域名或 IP。

## 无密码登录

1. 生成公钥：`ssh-keygen`
2. 复制公钥到远程服务器：

```bash
ssh user_name@remote_host "cat >> ~/.ssh/authorized_keys" < ~/.ssh/id_rsa.pub
```

## 指定端口

```bash
ssh -p port_number user_name@remote_host
```

## 图形化转发

```bash
ssh -X user_name@remote_host "command"
```

远程运行命令，本地显示图形界面。

## 远程执行命令

```bash
ssh user_name@remote_host "command" > local_file
```

同步等待远程命令完成，输出重定向到本地文件。返回码为远程命令返回码。

## sshpass

自动传递密码，适用于自动化脚本：

```bash
sshpass -p password ssh user_name@remote_host "command"
```

## SCP

SSH 协议的文件传输工具，用法与 SSH 相似。

## 相关概念

- [[Tmux]] - 终端复用，配合 SSH 使用
- [[Socket编程]] - 网络通信
- [[Vim]] - 远程编辑

---

**参考来源**：[ssh_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/ssh_note)