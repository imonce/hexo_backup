---
title: '[Trick]-bash: tensorboard: 未找到命令'
date: 2018-09-06 10:51:44
tags: [linux, trick, tensorboard]
comments: true
---

# 原因

tensorboard命令不在环境变量中

# 解决思路

找到tensorboard脚本路径，然后运行

# 解决方法


```bash
python3 /home/USERNAME/.local/lib/python3.6/site-packages/tensorboard/main.py --logdir=LOGDIR
```

***NOTE：***
- USERNAME是指用户名称
- LOGDIR是指log文件存放的相对或绝对目录