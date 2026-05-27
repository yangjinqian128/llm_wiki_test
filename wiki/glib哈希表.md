---
title: glib哈希表
tags: [c, glib, hash, data-structure]
created: 2026-05-22
source: raw/notes/algorithm/hash_note
---

# glib哈希表

glib(GNOME lib)提供的哈希表[^1]。

## 安装

```bash
sudo apt-get install libglib2.0-dev
gcc test.c `pkg-config --cflags --libs glib-2.0`
```

## 使用方法

```c
GHashTable *hash_table;

// 创建，提供hash函数和equal函数
hash_table = g_hash_table_new_full(key_hash, key_equal, g_free, g_free);

// 添加
g_hash_table_insert(hash_table, key, value);

// 查找
value = g_hash_table_lookup(hash_table, key);

// 销毁
g_hash_table_remove_all(hash_table);
```

## 自定义key

```c
typedef struct key {
    int bus;
    int devfn;
} key;

guint key_hash(gconstpointer v) {
    key *k = (key *)v;
    return hash_func(k->bus, k->devfn);
}

gboolean key_equal(gconstpointer v1, gconstpointer v2) {
    key *k1 = (key *)v1;
    key *k2 = (key *)v2;
    return (k1->bus == k2->bus) && (k1->devfn == k2->devfn);
}
```

## 简化方式

```c
// int作为key
h = g_hash_table_new(g_direct_hash, g_direct_equal);
g_hash_table_insert(h, GINT_TO_POINTER(int_key), value);
g_hash_table_lookup(h, GINT_TO_POINTER(int_key));
```

## 相关概念

- [[uthash哈希表]]
- [[TAILQ链表]]
- [[优先级队列]]

[^1]: raw/notes/algorithm/hash_note