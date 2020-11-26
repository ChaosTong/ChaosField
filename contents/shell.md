---
title: 常用 shell command
abbrlink: shell_command
date: 2020-11-21 11:58:11
abstract: shell command
tags:
  - command
---

## 常用 shell 命令

```shell
# 返回进入此目录之前所在的目录
cd -

(?)一个字符
(*)0个或多个字符

# 查看文件/目录
ls
    -alF
    -F  显示目录
    -a  显示隐藏文件
    -R  递归显示所有文件
    -i  inode num
    -l  显示长列表
    -l  Developer
    -l  Deve?oper
    -l  Deve*
    -l  D[eo]*      e 或者 o
    -l  D[a-z]*     a到z

# 复制文件/文件夹
cp source destination
    -R  递归复制=文件夹
    -i  防止直接覆盖

# 链接
ln source_file link_name
    -s  符号链接(软链接) = 快捷方式
        硬链接 = 同一文件

# 移动 重命名
mv source target
    -i  覆盖提示
    -v  verbose

# 删除文件
rm
    -i  f?ll 防止误删
    -f  强制不提示
    -rR 递归
    -ir 删文件夹 询问

# 文件夹创建
mkdir
    -p  同时创建多个目录和子目录

# 删除空目录
rmdir

# 查看文件类型
file
```

---

## 文件查看

```shell
# 查看完整文件
cat
    -n  显示行号
    -b  空行不显示行号

# 查看部分文件
more    分页, 只显示一页可以翻页
less is more    高级版的 more

# 查看部分文件 尾部
tail
    -f  一直监听
    -n 2 log_file   查看最后两行

# 查看部分文件 头部
head
    -5 log_file     查看头5行
```

## 更多的 shell 命令

```shell
# 探查进程
ps
UNIX 风格
    -e  查看所有进程
    -f  显示完整格式的输出

# 实时监测进程
top

# 结束进程
kill PID
killall NAME

# 监测磁盘空间
mount
umount

df
    -h  用户易读格式输出

du
    -s  显示每个输出参数的总计
    -h

# 处理数据文件
# 排序
sort
    -n  按数字排序
    -r  降序

# 文件大小排序
du -sh * | sort -nr

# 搜索数据
grep [options] pattern [file]
    -v  反向搜索
    -n  显示行号
    -c  统计匹配数量
    -e  多个匹配模式

# 压缩数据
gzip
# 查看压缩文件内容
gzcat
# 解压文件
gunzip

# 归档数据
tar
    -zxvf filename.tgz  解压 *.tgz* 文件 归档 gzip 文件
    -cvf test.tar test/ test2/
    -tf test.tar
    -c  创建归档文件
    -t  列出已归档文件
    -v  处理文件时显示文件
    -x  提取文件
    -f  输出文件
    -z  输出重定向给 gzip 来压缩内容
```

## Linux 环境变量

- 全局环境变量: shell 会话和所有生成的子 shell 都是可见的
- 局部环境变量: 只对创建它们的 shell 可见

```shell
# 查看环境变量
env
# 设置全局环境变量
export https_proxy=http://127.0.0.1:6152;export http_proxy=http://127.0.0.1:6152;export all_proxy=socks5://127.0.0.1:6153
# echo $PATH 显示
/Users/chaostong/.cargo/bin:/opt/MonkeyDev/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/Apple/usr/bin:/Users/chaostong/.rvm/bin:/Users/chaostong/miniconda2/bin
# 设置 PATH 引用原值
PATH=$PATH:/usr/local/bin/fastlane
```

### 环境变量持久化

- 交互式 shell 进程: 不是登录系统时启动的, 启动时只会检查用户 `HOME` 目录的 `.zshrc` 文件

`shell` 进程杀掉后 全局环境变量还是没了, 持久化需要写入 `~/.zshrc` 中, 然后 `source` 或者重启 `shell` 进程

## 文件权限

### 第一位

- `-` 文件
- `d` 目录
- `l` 链接
- `c` 字符型设备
- `b` 块设备
- `n` 网络设备

### 第二位 三组字符

第一个是文件属主的权限, 第二个是属组成员的权限, 第三个是其他用户权限

- `r` 可读
- `w` 可写
- `x` 可执行

### 默认文件权限

```shell
touch fuck.md
ll fuck.md
-rw-r--r--  1 chaostong  staff     0B Nov 25 15:58 fuck.md
# 默认权限设置
umask
# 输出 022, 3位八进制 属主 属组 其他用户, 644 就是 022 对应的文件默认权限
# 对文件来说 666(全权限值) - 022 = 644, 文件夹是 777 - 022 = 755

# 改变权限
chmod options mode file
chmod 760 fuck.md
```
