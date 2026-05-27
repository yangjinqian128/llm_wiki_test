---
title: TAILQ链表
tags: [c, data-structure, queue, bsd]
created: 2026-05-22
source: raw/notes/algorithm/TAILQ_note
---

# TAILQ链表

BSD实现的简单链表，include sys/queue.h[^1]。

## 数据结构

```c
struct your_list_node {
    int data;
    TAILQ_ENTRY(your_list_node) node;  // 嵌入节点结构
};

TAILQ_HEAD(list_head, your_list_node);  // 定义链表头类型
```

TAILQ_ENTRY定义前后节点指针结构[^1]。

## 基本操作

```c
struct list_head head;
TAILQ_INIT(&head);                       // 初始化

struct your_list_node e1;
TAILQ_INSERT_TAIL(&head, &e1, node);     // 插入尾部

TAILQ_FOREACH(tmp, &head, node) {        // 遍历
    printf("%d\n", tmp->data);
}

TAILQ_INSERT_BEFORE(&e4, &e5, node);     // 前插

tmp = TAILQ_LAST(&head, list_head);      // 获取末尾

TAILQ_REMOVE(&head, &e2, node);          // 删除
```

## 相关概念

- [[优先级队列]]
- [[uthash哈希表]]
- [[C语言宏]]

[^1]: raw/notes/algorithm/TAILQ_note