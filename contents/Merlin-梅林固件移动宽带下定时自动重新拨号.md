---
title: Merlin 梅林固件移动宽带下定时自动重新拨号
abstract: 脚本执行定时自动重新拨号
tags:
  - 路由器
abbrlink: 35e54d51
date: 2019-11-12 10:57:06
---
> 移动宽带一次拨号时间长了, 会有内网 IP 的情况下却没有网络的状态, 这时候重新拨号可以解决这个问题. 但是这个薛定谔的没网状态我也不知道什么时候发生, 于是写了一个脚本每天凌晨自动重新拨一次号来解决这个问题.
> 路由器是 R6300V2, 梅林版本 380.70_0-X7.9.1
> [参考链接](https://bugxia.com/946.html)

首先在 *系统管理* -> *系统设置* 中启用 JFFS 功能, 相信刷这个固件的这个功能都是开启的了.
在 /jffs/scripts/ 下创建文件 init-start , 启动命令内容如下:

``` shell
#!/bin/sh
sh /koolshare/scripts/auto_wan.sh
```

保存后执行 *chmod a+rx /jffs/scripts/init-start*
真实的脚本文件存放在了 /koolshare/scripts/ 路径下了, 这里的作用是在路由器重启的时候执行此文件的内容.

auto_wan.sh 脚本中的内容是一条定时任务, 定时重新拨号

``` shell
#!/bin/sh
echo "00 08 * * * service restart_wan #autoWan#" >> /var/spool/cron/crontabs/admin
```

意思是每天 8:00 重新拨号. 重启一下路由器, 执行 *crontab -e* 查看一下定时任务有没有加载, 就大功告成了, 宽带就不会再掉线了, 应该吧.
