---
title: 'latex:统一图表caption格式'
date: 2018-12-27 15:18:03
tags: [latex, caption]
---

# 方法

通过caption包中的captionsetup来进行格式的统一设定

## 栗子

```
\usepackage{caption}
\captionsetup{
   labelsep = quad,
   justification = raggedright,
   font = {singlespacing,sf},
   singlelinecheck=off,
   skip=4pt,
   position=top}
```

Captionsetup中具体字段含义及修改方法见：https://blog.csdn.net/stereohomology/article/details/37741591