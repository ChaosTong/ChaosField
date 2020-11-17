---
title: 一台全新的macOS都需要装啥(开发者-其实是我自己啦)
abstract: 都需要装什么呢?
tags:
  - developer
  - macbook
  - software
abbrlink: 517a9e52
date: 2019-07-31 22:18:18
---

## Xcode

iOS开发必备, 顺便把**Git**给装了. 商店可下载, [beta 地址](https://developer.apple.com/download/)

- 证书导入...
- .ssh/ 文件夹 id_rsa 私钥拷贝, 省的我多台服务器要添加那啥, authorized_keys 公钥填一下, 方便远程管理

## 代理

- [ShadowsocksX-NG-R](https://github.com/qinyuhang/ShadowsocksX-NG-R/releases) Mac SSR
- [Surge Mac 2.6.7](https://www.nssurge.com/mac/Surge-2.6.7-656.zip) 我的License只支持这个版本了, V3版本换个UI好贵了, 算了又不是不能用.

## 命令行工具

- [iTerm](https://iterm2.com/), 自带的**terminal**就哪凉快哪呆着吧.  
- [Oh My Zsh](https://ohmyz.sh), zsh从10.15 Catalina替代了原有的bash, 大快人心, 原因是开源协议的问题吧. 这个东西呢, 就是让zsh更好用, 帮助你配置了一下

``` shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

- 主题、字体安装...日后配置吧

## 命令行软件

- [Homebrew](https://brew.sh/index_zh-cn) macOS 缺失的软件包的管理器
- [CocoaPods](https://cocoapods.org) iOS开发必备

```
sudo gem install cocoapods
```

- [Node.js](https://nodejs.org/en/download/) 
- [npm](https://www.npmjs.com)

``` shell
brew install node
```

- [cnpm](https://npm.taobao.org)

``` shell
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

## 日常必备软件

- [Paste](https://pasteapp.me)
- [Alfred](https://www.alfredapp.com) 键盘改键, spotlight 快捷键启动 Alfred
- [VSCodium](https://vscodium.com) **VSCode** 的纯净无添加版本. 搭配[SynthWave '84](https://github.com/robb0wen/synthwave-vscode)妖艳的配色

## Docker

``` shell
brew cask install docker
```

使用[DaoCloud](https://www.daocloud.io/mirror)设置国内镜像加速
