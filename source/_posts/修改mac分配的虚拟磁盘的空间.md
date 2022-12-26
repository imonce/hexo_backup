---
title: 修改mac分配的虚拟磁盘的空间
date: 2022-08-22 10:20:01
tags: [mac]
---

通过mac自带的磁盘工具调整虚拟磁盘大小会报错，但是可以通过命令行调整。

如将Hello.dmg调整为7GB：

```
hdiutil resize -size 7g Hello.dmg
```