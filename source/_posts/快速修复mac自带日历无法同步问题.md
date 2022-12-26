---
title: 快速修复mac自带日历无法同步问题
date: 2022-08-22 10:16:54
tags: [mac]
---

打开terminal运行以下指令：

```
launchctl stop com.apple.CalendarAgent
launchctl start com.apple.CalendarAgent
```