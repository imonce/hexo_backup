---
title: 如何把jupyter notebook自动转换为python脚本
date: 2021-11-21 11:45:59
tags: [Python]
---

将YOURNOTEBOOKNAME替换为需要转换的文件名称，运行以下命令即可

```sh
jupyter nbconvert --to script YOURNOTEBOOKNAME.ipynb
```

如果使用了anaconda环境，且未将其设置为默认环境的话，可以在jupyter命令前补充安装路径。

以安装路径为“~/opt/anaconda3”为例：

```sh
~/opt/anaconda3/bin/jupyter nbconvert --to script YOURNOTEBOOKNAME.ipynb
```
