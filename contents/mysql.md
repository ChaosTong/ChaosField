---
title: mysql 8 on macOS
abbrlink: mysql_on_mac
date: 2020-11-18 11:41:10
abstract: mysql tips
tags:
  - mysql
---
> 环境 macOS: 11.0.1
参考 link :

1. [MySQL CentOS7 手动安装](https://www.cnblogs.com/jonney-wang/p/11279220.html)
2. [MySQL 8.0.11 error connect to caching_sha2_password the specified module could not be found](https://stackoverflow.com/questions/50169576/mysql-8-0-11-error-connect-to-caching-sha2-password-the-specified-module-could-n)
3. [mysql 创建root用户和普通用户 修改,删除](https://blog.csdn.net/u014745198/article/details/71554310)

## 安装

[下载地址](https://dev.mysql.com/downloads/mysql/)
选最小的文件下载

```shell
# 解压
tar -zxvf mysql-8.0.22-macos10.15-x86_64.tar.gz
# 移动到 /usr/local/mysql 下, 并重命名文件见
mv mysql-8.0.22-macos10.15-x86_64 /usr/local/mysql
mv mysql-8.0.22-macos10.15-x86_64 mysql-8.0.22
# 更改读写权限 防止启动服务权限报错
chmod -R 777 /usr/local/mysql/mysql-8.0.22
```

## 准备 my.cnf 配置文件

保存至 `/usr/local/mysql/mysql-8.0.22/` 下

```shell
[mysqld]

port=3306
basedir=/usr/local/mysql/mysql-8.0.22
datadir=/usr/local/mysql/mysql-8.0.22/data
pid-file=/usr/local/mysql/mysql-8.0.22/mysqld.pid
log-error=/usr/local/mysql/mysql-8.0.22/mysqld.err

user = root
max_connections = 151
symbolic-links = 0
lower_case_table_names = 1
character-set-server = utf8
collation-server=utf8_general_ci
bind-address = 0.0.0.0
socket=/usr/local/mysql/mysql-8.0.22/mysql.sock

[client]
port=3306
socket=/usr/local/mysql/mysql-8.0.22/mysql.sock

default-character-set=utf8
```

## 修改 mysql.server 文件

修改 `support-files/mysql.server` 文件

```shell
# 修改 basedir、datadir
basedir=/usr/local/mysql/mysql-8.0.22
datadir=/usr/local/mysql/mysql-8.0.22/data
```

## 环境变量修改

```shell
# vi ~/.zshrc
MYSQL_HOME=/usr/local/mysql/mysql-8.0.22
PATH=$PATH:$MYSQL_HOME/bin
```

然后执行 `source ~/.zshrc`

## 启动服务

```shell
# 得到初始随机密码
./bin/mysqld --user=root --basedir=/usr/local/mysql/mysql-8.0.22 --datadir=/usr/local/mysql/mysql-8.0.22/data --initialize
#开启MySQL服务
./support-files/mysql.server start
# 以初始密码登陆
mysql -u root -p
# 登陆后修改初始密码
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
# 开启远程访问
use mysql
select host,user from user;
update user set host='%' where user='root';
flush privileges;
```

## 创建用户

```shell
用户管理
mysql>use mysql;
查看
mysql> select host,user,password from  user;
创建用户
mysql> insert into mysql.user (Host,User,Password) Values('%','wise',PASSWORD('passwd'));
msyql> flush privileges;
修改
mysql>rename user feng to newuser; //mysql 5之后可以使用，之前需要使用update 更新user表
删除
mysql> drop user newuser; //mysql5之前删除用户时必须先使用revoke 删除用户权限，然后删除用户，mysql5之后drop 命令可以删除用户的同时删除用户的相关权限
更改密码
mysql> set password for zx_root =password('xxxxxx');
mysql> update mysql.user set password=password('xxxx') where user='otheruser'
查看用户权限
mysql> show grants for zx_root;
赋予权限
mysql> grant all privileges on YQ.* to wise;
```

## 报错

`MySQL said: Authentication plugin 'caching_sha2_password' cannot be loaded: dlopen(/usr/local/lib/plugin/caching_sha2_password.so, 2): image not found`

```shell
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'xxx';
```
