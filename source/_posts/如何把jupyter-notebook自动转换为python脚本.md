---
title: 如何把jupyter notebook自动转换为python脚本
date: 2021-11-21 11:45:59
tags: [Python]
---

将YOURNOTEBOOKNAME替换为需要转换的文件名称，运行以下命令即可

```sh
jupyter nbconvert --to script YOURNOTEBOOKNAME.ipynb
```