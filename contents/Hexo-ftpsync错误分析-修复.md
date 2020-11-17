---
title: Hexo ftpsync错误分析&修复
abstract: '直接修改源码, 简单暴力'
tags:
  - hexo
  - deploy
  - ftp
  - ftpsync
abbrlink: hexo_ftpsync
date: 2019-07-18 12:30:25
---

## 使用ftp方式同步静态文件到虚拟主机过程中, committing部分卡住

可以参见原文查看办法和分析[Hexo ftpsync错误分析](https://codeplot.top/2019/05/12/Hexo-ftpsync错误分析/)我就提供下最方便的懒人直接操作了

其中有解决办法, 需要修改依赖中的代码, 本地调没事儿, 但是要是不在本地还得在改一次, 我就fork了一下源码, 重新从自己仓库里拉改过的版本就完事儿了.

可以将原本 package.json 中的*hexo-deployer-ftpsync*版本号, 改成这个仓库, 或者你可以自己改了走你自己仓库, whatever

``` json
"dependencies": {
    ...
    "hexo-deployer-ftpsync": "git+ssh://git@github.com:ChaosTong/hexo-deployer-ftpsync.git",
    ...
}
```

改完后*npm install*, 没生效的话, 可以把*node_modules*文件夹删了, 重新安装一下.
