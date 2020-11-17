---
title: 使用 MonkeyDev LogosTweak 模版 钉钉模拟定位 实现
abstract: 钉钉模拟定位任意位置打卡
tags:
  - iOS
  - 逆向
abbrlink: d19fd53c
date: 2019-11-14 10:15:22
---
> 环境: macOS Catalina, Xcode 11.2, iPhone 6 12.4 (checkra1n 不完美越狱)
> 前提: Mac 安装 [MonkeyDev](https://monkeydev.org), iOS 通过 Cydia 安装 OpenSSH, mobilesubstrate 
> [辅助文章](https://www.jianshu.com/p/a62efc01995c)
> [辅助文章2](https://github.com/jackrex/FakeWeChatLoc)

[checkra1n](https://checkra.in) 使用 [checkm8](https://twitter.com/axi0mX/status/1177542201670168576) 芯片漏洞, 可以在 iPhone 5s - iPhone X 之间的所有设备和以后的所有升级系统中越狱, 截止 19.11.12 发布的 0.9.2 beta 版本只能运行在 macOS 上, 并且是不完美越狱每次重启需要连接电脑来重新引导再次越狱.

使用到的代码, 就只有这一段, 需要什么定位自行修改

``` objectivec
// See http://iphonedevwiki.net/index.php/Logos

#if TARGET_OS_SIMULATOR
#error Do not support the simulator, please use the real iPhone Device.
#endif

#import <UIKit/UIKit.h>
#import <CoreLocation/CoreLocation.h>

%hook CLLocation
-(CLLocationCoordinate2D) coordinate
{
    CLLocationCoordinate2D location;
    //纬度
    location.latitude = 26.012345;
    //经度
    location.longitude = 106.49328;
    return location;
}
%end
```

## 注意事项

1. 需要修改 build settings 中的设备 IP Port 和 密码, 钉钉 Bundle ID = com.laiwang.DingTalk
2. 报错 “error: An empty identity is not valid when signing a binary for the product type ‘Dynamic Library’. (in target ‘xxx’)”
    在Xcode中该 target 的 build settings 中添加"CODE_SIGNING_ALLOWED=NO", 问题解决.

## 总结

处理好以上问题, 出现其他问题无非是开发环境或者 iOS 软件依赖的问题. 可以随时随地愉快的打卡了
