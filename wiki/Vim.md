---
title: Vim
aliases: [vim, vim编辑器]
tags: [编辑器, vim, 开发工具]
source: raw/notes/vim_note
---

# Vim

Linux 下最强大的文本编辑器，支持丰富的插件和自定义配置。

## 模式切换

| 操作 | 说明 |
|------|------|
| `Ctrl+[` | 切换到普通模式（比 Esc 更便捷） |
| `i` | 进入插入模式 |
| `R` | 进入替换模式（覆盖字符） |
| `gR` | 虚拟替换模式（按实际占位替换） |

## 插入模式删除

| 操作 | 说明 |
|------|------|
| `Ctrl+w` | 删除当前单词 |
| `Ctrl+u` | 删除到行首 |

## 光标移动

| 操作 | 说明 |
|------|------|
| `z+Enter` | 当前行拉到屏幕顶部 |
| `f+char` | 跳到本行第一个指定字符 |
| `t+char` | 光标移动到指定字符前一个位置 |
| `Ctrl+d/u` | 上下翻半页 |
| `Ctrl+e/y` | 光标不动，上下滚屏 |
| `[[/]]` | 跳到函数开头（前/后） |
| `[]/][` | 跳到函数结尾（前/后） |
| `Ctrl+i/o` | 跳转光标历史位置（前/后） |

## 文本操作

| 操作 | 说明 |
|------|------|
| `daw` | 删除当前单词 |
| `cw` | 删除到单词结尾并进入插入 |
| `caw` | 删除整个单词并进入插入 |
| `.` | 重复上一个操作 |
| `gU` | 可视模式下转大写 |

## 搜索替换

| 操作 | 说明 |
|------|------|
| `%s/xxx/yyy/g` | 全局替换 |
| `XX,XXs/a/b/g` | 指定行范围替换 |
| `XX,XXs/a/b/gc` | 逐个确认替换 |
| `XX,XXs/^/#/g` | 行首添加注释 |

## Visual 模式

| 操作 | 说明 |
|------|------|
| `Ctrl+v` | 块选择 |
| `Shift+v` | 行选择 |

批量注释：`Ctrl+v` 向下选中 → `I` 输入注释符 → `Esc`

## 窗口操作

| 操作 | 说明 |
|------|------|
| `vert term` | 水平创建终端 |
| `Ctrl+w+N` | 终端进入普通模式 |

## 宏录制

```
q[a-z]  开始录制宏
q       停止录制
@[a-z]  执行宏
```

## 代码补全与跳转

| 操作 | 说明 |
|------|------|
| `Ctrl+p` | 代码补全 |
| `:ts 符号` | 查看符号定义列表 |
| `:tj 符号` | 跳转到符号 |
| `:stj 符号` | 分屏跳转到符号 |

## Tags 配置

```vim
set tags=tags;  " 逐级向上查找tags
set tags=/path1/tags,/path2/tags  " 多tags文件
```

## 常用插件

### easymotion

快速光标跳转插件：

```vim
" 安装
Plugin 'easymotion/vim-easymotion'

" 配置
nmap ss <Plug>(easymotion-s2)
```

使用：按 `ss` → 输入两个字符 → 按标记字母跳转。

### drawIt

ASCII 图绘制插件，用于绘制表格和流程图。

### NERDTree / TlistToggle

```vim
noremap <F4> :TlistToggle<CR>  " 函数/变量列表
noremap <F2> :NERDTreeToggle<CR>  " 目录树
nnoremap <F3> :Grep<CR>  " 搜索
```

## 配置示例

```vim
set cc=80  " 第80列显示红线
set tabstop=8  " Tab缩进8字符
set spell  " 拼写检查
```

## 终端内操作

| 操作 | 说明 |
|------|------|
| `:vimgrep /pattern/ **/*.[ch]` | 搜索代码 |
| `:copen` | 打开 quickfix 列表 |
| `:cclose` | 关闭 quickfix 列表 |

## ASCII 图工具

### boxes

```bash
echo "example" | boxes -d dog
```

### drawIt

先输入字符 → 打开 drawIt → 自动添加线条。

## 相关概念

- [[Git基础操作]] - Git 与 Vim 配合使用
- [[GDB]] - 调试时使用 Vim 查看
- [[Tmux]] - 终端复用与 Vim

---

**参考来源**：[vim_note](file:///home/yangjinqian/note/llm_wiki_test/raw/notes/vim_note)