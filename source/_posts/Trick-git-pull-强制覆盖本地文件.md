---
title: '[Trick]git pull 强制覆盖本地文件'
date: 2018-07-31 12:50:16
tags: [git, Trick]
comments: true
---

```git
git fetch --all 
git reset --hard origin/master
git pull
```

note：出错的话就再试一次，说不定就可以了