---
title: 'Python快速入门：6小时精通Python(四)'
date: 2019-04-19 17:35:12
tags: [6小时精通Python, Python, Python3, Python精通]
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
## 4. Functions
####################################################
```


```python
# 通过def保留字来定义函数
def add(x, y):
    print("x is {} and y is {}".format(x, y))
    # return语句用来返回处理结果
    return x + y  
```


```python
# 定义之后可以带参调用
c = add(5, 6)
print(c)
```

    x is 5 and y is 6
    11



```python
# 如果不按照顺序输入参数，需要添加形式参数名称
c = add(y=6, x=5)
print(c)
```

    x is 5 and y is 6
    11



```python
# 也可以传入参数列表（positional arguments）
def varargs(*args):
    print(type(args))
    return args
```


```python
c = varargs(1, 2, 3)
print(c)
```

    <class 'tuple'>
    (1, 2, 3)



```python
# 可以通过keyword arguments来传入多个变量
def keyword_args(**kwargs):
    print(type(kwargs))
    return kwargs
```


```python
c = keyword_args(one='1', two='2')
print(c)
```

    <class 'dict'>
    {'one': '1', 'two': '2'}



```python
# 也可以混合使用
def all_the_args(*args, **kwargs):
    print(args)
    print(kwargs)

"""
all_the_args(1, 2, a=3, b=4) prints:
    (1, 2)
    {"a": 3, "b": 4}
"""
```


```python
# 调用函数的时候，*和**也可以反过来使用
args_call = (1, 2, 3, 4)
kwargs_call = {"a": 3, "b": 4}
all_the_args(*args_call)            # equivalent to all_the_args(1, 2, 3, 4)
all_the_args(**kwargs_call)         # equivalent to all_the_args(a=3, b=4)
all_the_args(*args_call, **kwargs_call)  # equivalent to all_the_args(1, 2, 3, 4, a=3, b=4)
```

    (1, 2, 3, 4)
    {}
    ()
    {'a': 3, 'b': 4}
    (1, 2, 3, 4)
    {'a': 3, 'b': 4}



```python
# 一个函数可以同时返回多个值
# 多个值是以不带括号的tuple的形式返回的
# 但是加了括号也没关系
def swap(x, y):
    return y, x
```


```python
x = 1
y = 2
x, y = swap(x, y)     # => x = 2, y = 1
(x, y) = swap(x, y)   # 这一句和上一句一样
```


```python
# 函数范围 
# 这里x是一个全局变量（global）
x = 5

def get_x(num):
    # 函数内部可以访问外部全局变量
    print(num)
    print(x)   # => 5

def set_x(num):
    # 但是不能在函数内部改变全局变量
    # 这里的x是一个新生成的，只在函数内生效的局部变量
    x = num    # => 43
    print(x)   # => 43

def set_global_x(num):
    # 如果想要在函数内部改变全局变量，需要通过global声明
    global x
    print(x)   # => 5
    x = num    # global var x is now set to num
    print(x)   # => num
```


```python
get_x(6)
```

    6
    5



```python
set_x(6)
print(x)
```

    6
    5



```python
set_global_x(6)
print(x)
```

    5
    6
    6



```python
# python支持头等函数
# 简单来讲，return的函数就是上层函数的头等函数
def create_adder(x):
    # suber就是简单的嵌套定义了一个函数
    def suber(z):
        return x - z
    n = suber(5)
    # adder参与返回值，是头等函数
    def adder(y):
        return n + y
    return adder
```


```python
add_10_minus_5 = create_adder(10)
```


```python
add_10_minus_5(3)
```




    8




```python
# python也支持匿名函数
# (lambda <形式参数（列表）>: <return语句>)(<实参>)
(lambda x: x > 2)(3)                  # => True
```




    True




```python
(lambda x, y: x ** 2 + y ** 2)(2, 1)  # => 5
```




    5




```python
# 匿名函数，实际上也是可以命名的
check_greater_than_2 = lambda x: x > 2
```


```python
check_greater_than_2(4)
```




    True




```python
# 还有内建的高阶函数
# 通过map将[1, 2, 3]分别装入add_10_minus_5进行运算
# 返回结果包装成list
list(map(add_10_minus_5, [1, 2, 3]))
```




    [6, 7, 8]




```python
# max是python的内建方法，求参数中的最大值
max(1,2,3)
```




    3




```python
# 下面的写法就是就是对位结合，进行计算
# 相当于list(max(1,4), max(2,2), max(3,1))
list(map(max, [1, 2, 3], [4, 2, 1]))
```




    [4, 2, 3]




```python
# filter 可以把返回值为true的参数，返回出来
list(filter(lambda x: x > 5, [3, 4, 5, 6, 7]))  # => [6, 7]
```




    [6, 7]




```python
# 也可以根据对列表的理解，写出漂亮的map和filter
[add_10_minus_5(i) for i in [1, 2, 3]]
```




    [6, 7, 8]




```python
[x for x in [3, 4, 5, 6, 7] if x > 5]  # => [6, 7]
```




    [6, 7]




```python
# 也可以写出漂亮的字典或者集合
{x for x in 'abcddeef' if x not in 'abc'}  # => {'d', 'e', 'f'}
```




    {'d', 'e', 'f'}




```python
{x: x**2 for x in range(5)}  # => {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```




    {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}



