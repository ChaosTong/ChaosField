---
title: 常用 vi 操作
abbrlink: vi
date: 2020-11-22 23:04:46
abstract: vi command
tags:
  - vi
---

## 常用 vi 操作

```shell
# vi/vim 禁用方向键
map <Left> <Nop>
map <Right> <Nop>
map <Up> <Nop>
map <Down> <Nop>

# 只有以上的配置会使得在插入模式下方向键仍可用，因为 map 只针对普通模式和可视模式下作了映射，故添加：

imap <Left> <Nop>
imap <Right> <Nop>
imap <Up> <Nop>
imap <Down> <Nop>
```

by the way, *.vimrc* 修改后不需要 source, 保存即生效
