---
title: 远程连接mysql报错：1130 - Host '192.168.2.204' is not allowed to connect to this MySQL server
date: 2017-04-24 02:45:19
tags: [mysql, 远程连接报错, trick]
comments: true
---

## 问题原因

MySQL自带配置数据库mysql中的表user中，User=root一栏，Host的值为localhost，导致root用户只能通过本地登录。

## 解决思路

将User=root对应行的Host一栏的值修改为`%`，允许任意ip登录root。

## 具体解决方案

在本机登入mysql后，更改 “mysql” 数据库里的 “user” 表里的 “host” 项，从”localhost”改称'%'即可 

```sql
mysql -u root -p  
mysql>use mysql;  
mysql>update user set host = '%' where user ='root';  
mysql>flush privileges;
```