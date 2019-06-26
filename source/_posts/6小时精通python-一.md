---
title: 'python快速入门：6小时精通python(一)'
date: 2019-04-19 17:34:59
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
## 1. 简单数据类型和运算
####################################################
```


```python
# 数学运算
1 + 1
```




    2




```python
# 除法默认会返回float（与python2不同）
10 / 2
```




    5.0




```python
# 除法取整(乡下取整)
5 // 3
```




    1




```python
-5 // 3
```




    -2




```python
# 如果需要返回float，至少需要有一个参与运算的数字是float
5 // 3.0
```




    1.0




```python
# 余数运算
7 % 3
```




    1




```python
# 幂运算
2 ** 3
```




    8




```python
# # & | ^ ~是按位运算符，这里不讲了
# # << >> 是移位运算符，这里就不展示了
# a = 0011 1100

# b = 0000 1101

# -----------------

# a&b = 0000 1100

# a|b = 0011 1101

# a^b = 0011 0001

# ~a  = 1100 0011
```


```python
# 使用括号强制优先
(1 + 3) * 2
```




    8




```python
# Boolean值有保留字
True
False
```


```python
# 取反用关键字not
not True
not False
```


```python
# 逻辑运算 and or
True and False
```




    False




```python
True or False
```




    True




```python
# 参与数字运算的时候 True默认为1 False为0
True + True # => 2
True * 8    # => 8
False - 5   # => -5
```


```python
# 与数字进行比较运算时，也按照1 0 来进行比较
0 == False  # => True
1 == True   # => True
2 == True   # => False
-5 != False # => True
```


```python
# 可以通过bool()，将int数值投射到bool值上
# 出了0是False，其他都是True
bool(0)     # => False
bool(4)     # => True
bool(-6)    # => True
```


```python
# 用boolean运算符直接对int数值进行计算，计算过程按照bool，返回的值依然是int
0 and 2     # => 0
-5 or 0     # => -5
```


```python
# 赋值是单等号 =，相等判断是双等号 ==
1 == 1  # => True
2 == 1  # => False
```


```python
# 不相等判断 !=
1 != 1  # => False
2 != 1  # => True
```


```python
# 数学比较
1 < 10  # => True
1 > 10  # => False
2 <= 2  # => True
2 >= 2  # => True
```


```python
# 判断2是否在一个范围内
1 < 2 and 2 < 3  # => True
4 < 2 and 2 < 5  # => False
# 也可以通过链式写法
1 < 2 < 3  # => True
4 < 2 < 5  # => False
```


```python
# 赋值是单等号 =，相等判断是双等号 ==
# 还有一个相等判断保留字 is
# is 判断前后两者是否指向同一个对象（如果是两个对象，就算值相同，也会返回False）
# == 只判断值是否相同
a = [1, 2, 3, 4]  # Point a at a new list, [1, 2, 3, 4]
b = a             # Point b at what a is pointing to
b is a            # => True, a and b refer to the same object
b == a            # => True, a's and b's objects are equal
b = [1, 2, 3, 4]  # Point b at a new list, [1, 2, 3, 4]
b is a            # => False, a and b do not refer to the same object
b == a            # => True, a's and b's objects are equal
```


```python
# 通过‘或者“可以创建string
"This is a string."
'This is also a string.'
```


```python
# String也可以通过+连接，但是尽量不要
"Hello " + "world!"  # => "Hello world!"
# 中间不写，也会自动连接
"Hello " "world!"    # => "Hello world!"
```




    'Hello world!'




```python
# 一个string可以看作是一个char的list
"This is a string"[0]  # => 'T'
```




    'T'




```python
# len()是一个保留函数，可以计算list的长度
len("This is a string")  # => 16
```




    16




```python
# python中的string对象，有.format方法，可以用来对该string进行格式化操作
"{} can be {}".format("Strings", "interpolated")  # => "Strings can be interpolated"
```




    'Strings can be interpolated'




```python
# 可以通过在大括号{}中添加format参数的index来进行填充指定
"{0} be nimble, {0} be quick, {0} jump over the {1}".format("Jack", "candle stick")
```




    'Jack be nimble, Jack be quick, Jack jump over the candle stick'




```python
# 也可以给format中的参数命名，来代替index.
"{name} wants to eat {food}".format(name="Bob", food="lasagna") 
```




    'Bob wants to eat lasagna'




```python
# 如果需要兼容python2，老版的format写法如下
"%s can be %s the %s way" % ("Strings", "interpolated", "old")
```




    'Strings can be interpolated the old way'




```python
# 在python3.6之后的版本中，可以在string前加f来进行format操作
name = "Reiko"
f"She said her name is {name}." # => "She said her name is Reiko"
# 在大括号中，也可以调用python的方法
f"{name} is {len(name)} characters long."
```




    'Reiko is 5 characters long.'




```python
# None也是一个对象，不是一个值
None
a1 = False
b1 = None
a1 is b1
```




    False




```python
# 不要用==来和None进行比较
# 要通过is来判断变量是不是None
"etc" is None  # => False
None is None   # => True
```




    True




```python
# None, 0, 以及空的 strings/lists/dicts/tuples 都等于 False.
# All other values are True
bool(0)   # => False
bool("")  # => False
bool([])  # => False
bool({})  # => False
bool(())  # => False
```




    False



