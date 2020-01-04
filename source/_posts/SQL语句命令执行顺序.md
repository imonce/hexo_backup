---
title: SQL语句命令执行顺序
date: 2019-11-06 22:42:22
tags: [SQL]
categories: [SQL]
---

```flow
op1=>operation: from + join
op2=>operation: where
op3=>operation: group by
op4=>operation: having
op5=>operation: select
op6=>operation: order by
op7=>operation: limit
op1->op2->op3->op4->op5->op6->op7
```