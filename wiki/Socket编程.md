---
title: Socket编程
aliases: [socket, TCP socket, UDP socket, Unix domain socket]
tags: [网络, socket, IPC]
source: raw/notes/UUP_socket_note
---

# Socket编程

进程间通信方式，支持本机和跨机器通信。

## IPC 方式对比

| 方式 | 适用场景 | 说明 |
|------|----------|------|
| 匿名管道 | 父子进程 | 单向通信 |
| 命名管道 | 独立进程 | 文件形式 |
| Unix domain socket | 本机进程 | 文件地址 |
| 共享内存 | 本机进程 | 高性能 |
| Socket | 跨机器 | TCP/UDP |

## TCP Socket

### Client 流程

```c
struct sockaddr_in servadd;

sock_id = socket(AF_INET, SOCK_STREAM, 0);

bzero(&servadd, sizeof(servadd));
hp = gethostbyname(hostname);  // 获取 IP
bcopy(hp->h_addr, &servadd.sin_addr, hp->h_length);
servadd.sin_port = htons(port);
servadd.sin_family = AF_INET;

connect(sock_id, (struct sockaddr *)&servadd, sizeof(servadd));
```

### Server 流程

```c
socket(PF_INET, SOCK_STREAM, 0);
bind(sock_id, (struct sockaddr *)&saddr, sizeof(saddr));
listen(sock_id, 1);

while (1) {
    sock_fd = accept(sock_id, NULL, NULL);
    sock_fp = fdopen(sock_fd, "w");  // 转为文件流
    // ... 处理
    fclose(sock_fp);
}
```

## UDP Socket（数据报）

无需建立连接，直接发送接收：

```c
sendto(sock, msg, len, 0, (struct sockaddr *)&addr, addrlen);
recvfrom(sock, buf, len, 0, (struct sockaddr *)&addr, &addrlen);
```

`recvfrom` 可获取发送者地址，用于回复。

## Unix Domain Socket

用文件路径作为地址：

```c
struct sockaddr_un addr;
addr.sun_family = AF_UNIX;
strcpy(addr.sun_path, "/tmp/socket");

socket(PF_UNIX, SOCK_DGRAM, 0);
bind(sock, (struct sockaddr *)&addr, addrlen);
```

## 共享内存

```c
seg_id = shmget(KEY, SIZE, IPC_CREAT | 0777);
mem_ptr = shmat(seg_id, NULL, 0);

// 使用 mem_ptr

shmdt(mem_ptr);
shmctl(seg_id, IPC_RMID, NULL);
```

需要信号量保证同步。

## 文件锁

文件通信需使用文件锁保证一致性。

## select/poll

响应多个阻塞文件描述符。

参见[[IO多路复用]]。

## 相关概念

- [[IO多路复用]] - select/poll/epoll
- [[eventfd]] - 事件通知
- [[SSH]] - SSH 协议
- [[信号处理]] - 进程间通知

---

**参考来源**：[UUP_socket_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/UUP_socket_note)