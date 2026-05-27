---
title: Makefile
aliases: [make, makefile, GNU Make]
tags: [构建系统, make, 编译]
source: raw/notes/make_note
---

# Makefile

GNU Make 的构建描述文件，定义目标、依赖和构建命令。

## 基本逻辑

Make 根据依赖关系决定是否执行命令。Makefile 只有一个最终目标（第一个遇到的 target）。

## 目标类型

| 类型 | 说明 |
|------|------|
| 真目标 | 需要构建的文件 |
| 伪目标 | 不生成文件的操作（如 clean） |

伪目标定义：

```makefile
.PHONY: clean
clean:
	rm *.o
```

## 变量

### 定义方式

| 方式 | 说明 |
|------|------|
| `=` | 延后展开（deferred） |
| `:=` | 立即展开（immediate） |
| `?=` | 条件定义 |
| `+=` | 追加 |

### 自动变量

| 变量 | 说明 |
|------|------|
| `$@` | 目标集合 |
| `$<` | 依赖中的第一个 |
| `$^` | 依赖集合 |

## 变量展开时机

- 立即展开：解析 Makefile 时
- 延后展开：执行命令时

`:=` 的右值必须在定义之前存在。

## 函数

### patsubst

```makefile
$(patsubst %.o, %.c, a.o b.o c.o)  # 结果: a.c b.c c.c
```

### foreach

```makefile
names := a b c d
files := $(foreach n,$(names),$(n).o)  # 结果: a.o b.o c.o d.o
```

### call

```makefile
$(call expression,para1,para2)  # 参数传给 $(1)/$(2)/$(3)
```

### eval

```makefile
$(eval expression)  # 展开变量或函数
```

### info/error/warning

```makefile
$(info xxx)  # 打印信息
```

## 自定义函数

```makefile
define rule=
$(1): test.o
	gcc test.o -o $(1)
endef

$(foreach n,$(_all),$(eval $(call test_rule,$(n))))
```

## 条件语句

```makefile
ifeq ($(xxx), $(yyy))
# ...
else
# ...
endif
```

## 隐含规则

Make 自动推导规则，如 `.o` 目标自动依赖同名 `.c` 文件。

自定义模式规则覆盖隐含规则：

```makefile
%.o: %.c
	gcc --static -g -c $< -o $@
```

## 目标变量

限定变量作用范围：

```makefile
lspci: LDLIBS+=libxxx  # LDLIBS 只对 lspci 目标有效
```

## Force Target

强制执行目标：

```makefile
lib/$(PCILIB): $(PCIINC) force
	$(MAKE) -C lib all

force:
```

`force` 没有依赖和命令，Make 认为它每次都 update。

## Include

```makefile
-include lib/config.mk  # -表示忽略错误
```

## Export

```makefile
export  # 导出所有变量到子 Make
export A  # 只导出 A
```

## pciutils Makefile 分析

关键点：

1. `CFLAGS` 通过隐含规则自动加入编译
2. `$(shell ...)` 调用 shell 命令
3. `export` 后子 Make 可见所有变量
4. 多目标展开为多个独立规则
5. 目标变量限定变量作用域

## 相关概念

- [[CMake]] - 跨平台构建系统
- [[Meson]] - 新一代构建系统
- [[链接]] - 编译链接过程

---

**参考来源**：[make_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/make_note)