---
title: '[Python]对dict字典进行排序'
date: 2018-08-08 09:33:09
tags: [Python, Dict, 字典, Python入门]
---

# 代码
```python
#定义字典
dict = {'a':1, 'b':2, 'c':3, 'd':4, 'e':5}
#根据key进行排序
dict_sorted_by_key = sorted(dict.items(), key=lambda d: d[0])
#根据key进行反向排序
dict_sorted_by_key_reverse = sorted(dict.items(), key=lambda d: d[0], reverse=True)
#根据value进行排序
dict_sorted_by_value = sorted(dict.items(), key=lambda d: d[1])
#根据value进行反向排序
dict_sorted_by_value_reverse = sorted(dict.items(), key=lambda d: d[1], reverse=True)
```
# 示例
![](https://raw.githubusercontent.com/imonce/imgs/master/20180808100756.png)