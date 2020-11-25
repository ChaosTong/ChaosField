---
title: iOS Hook
abbrlink: iOS_hook
date: 2020-11-24 15:37:26
abstract: iOS Hook
tags:
  - iOS
  - hook
---

> [XCode 11 Monkey 报错](https://iosre.com/t/monkey/16017)
> [iOS 逆向 - 钉钉在家打卡插件实战](https://juejin.cn/post/6844904093790502919#heading-23)

## 准备

1. 越狱手机一台, 安装 *openssh*
2. 安装 [MonkeyDev](https://github.com/AloneMonkey/MonkeyDev/wiki/安装)

## 原文中 hook 代码调整

```shell
%hook LAPluginInstanceCollector
- (void)handleJavaScriptRequest:(NSDictionary *)arg1 callback:(void(^)(id))arg2{
    if([arg1[@"action"] isEqualToString:@"start"]){//有可能需要修改定位信息!
        //定义一个myBlock
        id myCallBack = ^(NSDictionary * block_arg){
            if([block_arg[@"keep"] isEqualToString:@"1"]){//需要修改GPS
                NSMutableDictionary * tempDic = [NSMutableDictionary dictionaryWithDictionary:block_arg];
                NSMutableDictionary * result = [NSMutableDictionary dictionaryWithDictionary:tempDic[@"result"]];
                //修改block中的字典的值!
                result[@"latitude"] = @"28.1924070001";
                result[@"longitude"] = @"112.9788130003";
                tempDic[@"result"] = result;
                //使用修改后的!
                arg2(tempDic);
            }else{
                //保持原有掉用!!
                arg2(block_arg);
            }
        };
        %orig(arg1,myCallBack);
    }else{
        %orig;
    }
}
%end
```
