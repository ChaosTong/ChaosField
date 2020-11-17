---
title: 使用 launchd 在 macOS 下设置开机未登录状态下自启软件
abstract: launchd macOS 启动服务管理
tags:
  - macOS
abbrlink: adeff96
date: 2019-11-12 10:56:39
---
> 环境 macOS: 10.15.1
参考 link :

1. [Launchd WiKi](https://en.wikipedia.org/wiki/Launchd)
2. [Launchd Info](https://www.launchd.info)

我的一个需求是在电脑重启后, 没有登录的状态下, 启动 frpc 从而达到我不可告人的用途, 呸. 就是希望他可以自启动啦, 还有就是公司的打包机器的 Jenkins 服务, 我要做的就是重启他, 不需要在远程登录启动服务, 懒·程序员的第一生产力.

本来我是有脚本启动 pm2 来控制这些进程的, 但是 node 服务在自启动上好像又有些别的幺蛾子, 我们就先直接单独启动 Jenkins 吧

## Jenkins 的自启动

> ps: 环境变量还是有问题, 任务无法执行...

这里的坑呢有一些, 首先未登录下的自启动用的是管理员账号也就是 root, 但是 Jenkins 安装配置文件夹 JENKINS_HOME 指定在一个 *.zshrc* 的位置, 通过 root 启动时并不会读区这个文件, 而是直接指定到了 /var/root/.jenkins 文件夹下了, 所以呢为了保持读写同一份配置我直接 ln 软链接了原本的配置文件夹到 root 下.
launchd 配置文件是 .plist 格式文件, 未登录的自启动配置文件位置在目录 /Library/LaunchDaemons 下, 需要注意把拥有者给 root:wheel 和变更读写权限为 644 通过执行命令

```shell
sudo chown root:wheel com.chaostong.jenkinsService.plist
sudo chmod 644 com.chaostong.jenkinsService.plist
```

格式去参考链接里看, Jenkins 的配置文件  :

``` plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>KeepAlive</key>
    <true/>
    <key>Label</key>
    <string>com.chaostong.jenkinsService</string>
    <key>ProgramArguments</key>
    <array>
        <string>/usr/local/bin/jenkins</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>WorkingDirectory</key>
    <string>/Users/chaostong</string>
</dict>
</plist>
```

文件写好了, 接下来就是加载他了, 使用命令 :

``` shell
# 加载
sudo launchctl load -w /Library/LaunchDaemons/com.chaostong.jenkinsService.plist
# 卸载
sudo launchctl unload -w /Library/LaunchDaemons/com.chaostong.jenkinsService.plist
# 查看服务状态
sudo launchctl list | grep chaos
# 会显示进程号 和 状态码, 进程号 非 - 表示启动了, 状态码 非 0 表示发生了错误
```

以上就可以让 Jenkins 服务自启动了, 这睿智软件让我我TM重启好多次调试啊.

## frpc 服务自启动

这个服务和上面那个有什么区别呢, 需要再写一大段了, 我直接改上面的启动命令换成 frpc 的启动命令和参数不就完事了吗? 不不不, 不一样, Jenkins 服务是一个单机的服务, 它是一个服务端, 没有网络的依赖启动后就可以通过 IP: PORT 访问它了, 但是 frpc 是客户端他需要网络去连接服务器, 如果直接修改使用上面的配置, 服务是正常启动了, 但是启动的时候机器网络还没有连上, 所以你把查看 frp web 的时候会发现怎么设备不在线, 尝试 sudo kill -9 pid 把进程杀掉后, 会自动重启服务, 然后发现设备在线了. 因此这个服务我是通过 launchd 启动一个脚本来检测网络通了后, 再通过脚本启动 frpc 服务.

launchd 配置:

``` plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>KeepAlive</key>
    <true/>
    <key>Label</key>
    <string>com.chaostong.remoteServices</string>
    <key>ProgramArguments</key>
    <array>
        <string>/bin/sh</string>
        <string>ping.sh</string>
    </array>
    <key>RunAtLoad</key>
    <true/>
    <key>WorkingDirectory</key>
    <string>/Users/chaostong</string>
</dict>
</plist>
```

其中的 ping.sh shell 脚本如下:

``` shell
#!/bin/bash
# ping 60 次, 间隔 1 s, 实际上失败的时候 ping 命令时间会比较长, 成功一次就直接跳出循环去启动 frpc 服务
ping_success_status() {
    if ping -c 1 $IP >/dev/null; then
        echo "$IP Ping is successful."
        cd /Users/chaostong/frp && ./frpc
        break
    else
        echo "$IP Ping is failed."
    fi
}
n=1
while (( $n <= 60 )); do
    (( n++ ))
    sleep 1
    IP="xx.xx.xx.xx"
    ping_success_status
done
```

使用上一步的加载命令加载此配置即可, 注意启动后的状态很容易出现 127 状态码, 就是 shell script 写的有问题啦.

## 总结

学会使用后, 就可以愉快的重启电脑不用手动启动有些烦人的服务了. 满足的我的需求我就不进阶学习了. 烦了我7、8个小时, 和重启了几十次验证, 至于启动 node 服务和 pm2 脚本执行为什么都 127 了, 我也不想研究了, frpc 都启动了, 剩下的手动登录服务器来处理下也是可以接受的嘛, 懒·程序员的第一生产力, 没有去实现只有两个原因, 实现不了, 懒得实现. 😊
