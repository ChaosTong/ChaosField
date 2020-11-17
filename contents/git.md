---
title: git
abstract: git tips
tags:
  - git
abbrlink: 518e617c
date: 2020-11-14 23:06:30
---

## .gitignore 文件

你可以在项目的任意目录下创建 .gitignore 文件，此文件的影响范围是当前目录和其所有子目录，可以在不同目录创建多个 .gitignore 文件.

## 删除已经跟踪的文件

```shell
git rm --cached test.txt # 如果是文件夹，后面再加上 -r
git commit -m 'delete test.txt'
```
