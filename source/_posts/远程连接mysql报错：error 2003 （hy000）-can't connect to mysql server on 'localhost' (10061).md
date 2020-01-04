---
title: 远程连接mysql报错：error 2003 （hy000）:can't connect to mysql server on 'localhost' (10061)
date: 2017-04-24 02:45:19
tags: [Mysql, 远程连接报错, Trick]
comments: true
---

## 问题原因

mysql配置文件中有一句：

`bind-address = 127.0.0.1`

导致mysql只能从本地进行连接。

## 解决思路

找到mysql的配置文件，将这一行注释掉。

## 具体解决方案

去两个配置文件中找这个配置项：

1. /etc/mysql/my.cnf
2. /etc/mysql/mysqld.cnf

在这两个文件的任意一个中找到

`bind-address = 127.0.0.1`

后，将其修改成：

`#bind-address = 127.0.0.1`

然后执行 `service mysql restart`重新启动mysql服务使配置生效即可。


