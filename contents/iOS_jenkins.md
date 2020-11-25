---
title: iOS Jenkins
abbrlink: iOS_jenkins
date: 2020-11-25 13:11:28
abstract: iOS Jenkins
tags:
  - iOS
  - jenkins
---

## 在 macOS 使用 Jenkins 通过 Fastlane 脚本打包 iOS App

```shell
# 安装 jenkins
brew install jenkins
brew services restart jenkins

# 需要外部访问的话需要修改 127.0.0.1 -> 0.0.0.0 然后重启服务
/usr/local/opt/jenkins/homebrew.mxcl.jenkins.plist
~/Library/LaunchAgents/homebrew.mxcl.jenkins.plist

# invalid byte sequence in US-ASCII (ArgumentError) 报错
LC_ALL=en_US.UTF-8
# 找不到 fastlane 命令, 需要添加 环境变量
echo $PATH
which fastlane
PATH=your path
```
