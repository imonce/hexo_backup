---
title: 'MacOS:如何在启动App时运行脚本'
date: 2024-07-08 11:04:51
tags: [macos]
mathjax: true
comments: true
---

1. 将主启动“APPNAME”程序更名为“APPNAME.real”，文件位置一般为应用包内的Contents/MacOS
2. 在同级目录下新建一个脚本文件，命名为“APPNAME”，执行`chmod a+x APPNAME`，这样应用启动时就会执行该脚本文件
3. 修改脚本内容，比如：

```
#!/bin/bash

### (其他在APP启动时需要执行的内容，比如删除文件：）
# rm -rf "/Users/$(whoami)/Library/Application Support/APPNAME"

"`dirname "$0"`"/APPNAME.real $@
```