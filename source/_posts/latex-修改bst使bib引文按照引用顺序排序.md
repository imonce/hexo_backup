---
title: 'latex:修改bst使bib引文按照引用顺序排序'
date: 2018-12-27 14:57:31
tags: [Latex, Latex Bib, Latex Bst]
comments: true
---

# 问题

使用spring的spbasic.bst模板，结果引文是按照作者名字的字母顺序排序的。

# 方法

1. 使用任意一款文本编辑器打开spbasic.bst文件
2. 找到其中所有的`SORT`行
3. 修改为`%SORT`即可

 注意大小写，spbasic.bst中应有两处

如果未生效，可能是bst编译的文件没有更新，试着把bbl文件删除，然后重新编译一下

# 解释

1. bst文件什么也不写的话默认是按照引用顺序来排序的
2. 很多模板中有一部分关于排序的函数，使其按照作者名字字母顺序排序
3. `SORT`行的作用就是调用这些方法，注释掉之后就变成了默认排序方法
