---
title: 6小时精通python(五)
date: 2019-04-19 17:35:16
tags: [6小时精通python, python, python3, python精通]
category: [Learn X in Y minutes, Learn Python in Y minutes]
---

译自：[https://learnxinyminutes.com/docs/python3/](https://learnxinyminutes.com/docs/python3/)

阅前须知：

- “#”后边的是注释
- 带行号的是python代码
- 不带行号的是代码的输出
- 把下边的语句对着敲一边自然就会了，博主用的是jupyter notebook

```python
####################################################
## 5. 模块
####################################################
```


```python
# 可以通过import语句导入模块（包）
import math
print(math.sqrt(16))  # => 4.0
```

    4.0



```python
# 也可以通过from import语句，从包中调用特定函数
from math import ceil, floor
print(ceil(3.7))   # => 4.0
print(floor(3.7))  # => 3.0
```

    4
    3



```python
# 也可以通过*，导入包中所有函数
# 不建议这样做，命名空间容易冲突（重名）
from math import *
```


```python
# 也可以通过import as语句来对包名进行缩写
import math as m
math.sqrt(16) == m.sqrt(16)  # => True
```




    True




```python
# Python包都是提前写好普通的python文件
# 也可以自己写，import名称为文件名
# 通过dir方法，可以看包中所有方法的directory
import math
dir(math)
```




    ['__doc__',
     '__file__',
     '__loader__',
     '__name__',
     '__package__',
     '__spec__',
     'acos',
     'acosh',
     'asin',
     'asinh',
     'atan',
     'atan2',
     'atanh',
     'ceil',
     'copysign',
     'cos',
     'cosh',
     'degrees',
     'e',
     'erf',
     'erfc',
     'exp',
     'expm1',
     'fabs',
     'factorial',
     'floor',
     'fmod',
     'frexp',
     'fsum',
     'gamma',
     'gcd',
     'hypot',
     'inf',
     'isclose',
     'isfinite',
     'isinf',
     'isnan',
     'ldexp',
     'lgamma',
     'log',
     'log10',
     'log1p',
     'log2',
     'modf',
     'nan',
     'pi',
     'pow',
     'radians',
     'sin',
     'sinh',
     'sqrt',
     'tan',
     'tanh',
     'tau',
     'trunc']




```python
# 如果你调用了一个自己写的包
# 其名称和内建包重复
# 则默认调用自己写的
```

