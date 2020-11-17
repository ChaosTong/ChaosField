---
title: 通过 USB 连接越狱设备
abstract: iproxy 链接越狱iOS设备
tags:
  - iOS
  - 逆向
abbrlink: c79e5e0f
date: 2019-11-12 10:56:55
---
``` shell
brew install libimobiledevice
iproxy 2222 22
ssh root@localhost 2222
```

# dumpdecrypted 通过 MonkeyDev 的简单使用

[魔改 dumpdecrypted](https://github.com/AloneMonkey/dumpdecrypted) 直接 Command+B 通过 MonkeyDev 可以复制到越狱机器中, 发生错误修改端口即可, 取决用你链接手机的方式和端口号.
需要在该项目中的 plist 文件中指定相应的 bundle id

[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump) run it , 有一点小错误, 但是解密后的文件还是解密出来了, 使用 Mac 上的 Console.app 过滤进程找到文件位置, 使用 scp 命令下载文件到电脑上吧

```shell
scp -P 2222 root@localhost:/var/mobile/Containers/Data/Application/7A423898-FA42-4B36-A158-25ED4173E2B2/Documents/WeChat.decrypted ./
WeChat.decrypted
```
