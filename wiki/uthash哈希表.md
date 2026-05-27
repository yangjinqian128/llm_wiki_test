---
title: uthash哈希表
tags: [c, hash, data-structure]
created: 2026-05-22
source: raw/notes/algorithm/hash_note
---

# uthash哈希表

uthash是用宏实现的哈希表[^1]。

## 使用方法

```c
struct my_struct {
    int id;
    char name[10];
    UT_hash_handle hh;        // 必须有，必须叫hh
};

struct my_struct *users = NULL;  // 表头初始化为NULL

// 添加
HASH_ADD_INT(users, id, s);      // 用s里的id为key

// 查找
HASH_FIND_INT(users, &user_id, s);

// 删除
HASH_DEL(users, user);

// 遍历
HASH_ITER(hh, users, current, tmp) {
    HASH_DEL(users, current);
    free(current);
}

// 排序
HASH_SORT(users, compare_func);

// 统计
HASH_COUNT(users);
```

## 注意事项

- id参数名要和结构体里的字段名一致[^1]
- 删除不释放内存，需手动free[^1]

## 相关概念

- [[glib哈希表]]
- [[TAILQ链表]]
- [[优先级队列]]

[^1]: raw/notes/algorithm/hash_note