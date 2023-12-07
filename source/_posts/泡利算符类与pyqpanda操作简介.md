---
title: 泡利算符类与pyqpanda操作简介
date: 2021-01-18 22:26:22
tags: [泡利算符, pyqpanda, Python, 量子计算]
categories: [量子计算]
mathjax: true
---

# 泡利算符类
泡利算符是一组三个2×2的幺正厄米复矩阵，又称酉矩阵。我们一般都以希腊字母 $\sigma$ 来表示。 在 QPanda 中我们称它们为X门，Y门，Z门。它们对应的矩阵形式如下。

X:
$\sigma_x = \left[\begin{matrix}
    0 & 1 \\\\
    1 & 0
\end{matrix}\right]$

Y:
$\sigma_y = \left[\begin{matrix}
    0 & -i \\\\
    i & 0
\end{matrix}\right]$

Z:
$\sigma_z = \left[\begin{matrix}
    1 & 0 \\\\
    0 & -1
\end{matrix}\right]$

每个抛离矩阵有两个特征值，+1和-1，其对应的归一化特征向量为：

$\psi_{x+}=\frac{1}{\sqrt{2}}\left[\begin{matrix}
    1 \\\\
    1
\end{matrix}\right]$


$\psi_{x-}=\frac{1}{\sqrt{2}}\left[\begin{matrix}
    1 \\\\
    -1
\end{matrix}\right]$


$\psi_{y+}=\frac{1}{\sqrt{2}}\left[\begin{matrix}
    1 \\\\
    i
\end{matrix}\right]$


$\psi_{y-}=\frac{1}{\sqrt{2}}\left[\begin{matrix}
    1 \\\\
    -i
\end{matrix}\right]$


$\psi_{z+}=\left[\begin{matrix}
    1 \\\\
    0
\end{matrix}\right]$


$\psi_{z-}=\left[\begin{matrix}
    0 \\\\
    1
\end{matrix}\right]$

# 泡利算符的运算规则

1. 泡利算符与自身相乘得到是单位矩阵
2. 泡利算符与单位矩阵相乘，无论是左乘还是右乘，其值不变
3. 顺序相乘的两个泡利算符跟未参与计算的泡利算符是i倍的关系
4. 逆序相乘的两个泡利算符跟未参与计算的泡利算符是−i倍的关系

# 接口介绍

```python
from pyqpanda import *

if __name__=="__main__":
    # 构造一个空的泡利算符类
    p1 = PauliOperator()

    # 2倍的"泡利Z0"张乘"泡利Z1"
    p2 = PauliOperator("Z0 Z1", 2)

    # 2倍的"泡利Z0"张乘"泡利Z1" + 3倍的"泡利X1"张乘"泡利Y2"
    p3 = PauliOperator({"Z0 Z1": 2, "X1 Y2": 3})

    # 构造一个单位矩阵，其系数为2，等价于p4 = PauliOperator("", 2)
    p4 = PauliOperator(2)
```

其中PauliOperator p2("Z0 Z1", 2)表示的是 $2\sigma_{0}^{z} \otimes \sigma_{1}^{z}$ ,这里的 $\otimes$ 表示[克罗内克积（Kronecker product）](https://zh.wikipedia.org/wiki/克罗内克积)，也称作张量积、张乘。

```python
from pyqpanda import *

if __name__=="__main__":

    a = PauliOperator("Z0 Z1", 2)
    b = PauliOperator("X5 Y6", 3)

    plus = a + b
    minus = a - b
    muliply = a * b

    print("a + b = ", plus)
    print("a - b = ", minus)
    print("a * b = ", muliply)

    print("Index : ", muliply.getMaxIndex())

    index_map = {}
    remap_pauli = muliply.remapQubitIndex(index_map)

    print("remap_pauli : ", remap_pauli)
    print("Index : ", remap_pauli.getMaxIndex())
```

![](https://raw.githubusercontent.com/imonce/imgs/master/20210118215817.png)

> reference:
> https://pyqpanda-toturial.readthedocs.io/zh/latest/PauliOperator.html
> https://www.bilibili.com/video/BV124411b7bd?p=3
