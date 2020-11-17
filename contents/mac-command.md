---
title: mac command
abstract: macOS tips
tags:
  - macOS
abbrlink: 41dcef00
date: 2020-11-14 22:52:38
---

## mac 命令

```shell
# 显示/隐藏 隐藏文件和文件夹
defaults write com.apple.finder AppleShowAllFiles -boolean true ; killall Finder
defaults write com.apple.finder AppleShowAllFiles -boolean false ; killall Finder
```

```shell
# 允许任何来源
sudo spctl --master-disable
# 提示 App 已损坏 绕过公证
sudo xattr -rd com.apple.quarantine /Applications/xxxx.app
```

## TimeMachine

```shell
# 用于访问非本机的 TimeMachine 备份文件
# 通过在Time Machine卷上设置“忽略所有权”，可以访问当前用户无法访问的文件夹。
vsdbutil -d '/Volumes/TimeMachineVolume'
```
