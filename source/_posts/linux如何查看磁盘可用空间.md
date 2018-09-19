---
title: linux如何查看磁盘可用空间
date: 2018-09-19 11:00:49
tags: [linux, 磁盘, 空间]
---

# 命令

> df -h

# 示例

```
USER_MANE@PC_NAME:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev             16G     0   16G   0% /dev
tmpfs           3.2G   26M  3.2G   1% /run
/dev/sda1       198G  151G   38G  81% /
tmpfs            16G  4.0K   16G   1% /dev/shm
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs            16G     0   16G   0% /sys/fs/cgroup
/dev/sdb1       917G  290G  581G  34% /SATA
tmpfs           3.2G  8.0K  3.2G   1% /run/user/1004
tmpfs           3.2G     0  3.2G   0% /run/user/1010
tmpfs           3.2G     0  3.2G   0% /run/user/1003
```