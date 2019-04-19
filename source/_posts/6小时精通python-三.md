---
title: 6小时精通python(三)
date: 2019-04-19 17:35:09
tags: [6小时精通python, python, python3, python精通]
---

译自：[https://learnxinyminutes.com/docs/python3/](https://learnxinyminutes.com/docs/python3/)

```python
####################################################
## 3. 控制流和迭代器
####################################################
```


```python
some_var = 5
# python通过缩进来对代码进行分段（连续同缩进量的代码可以看作在一个大括号里，空行、注释行自动忽略）
# 一个缩进应该是4个空格，不是制表符
if some_var > 10:
    print("some_var is totally bigger than 10.")
elif some_var < 10:    # 可选
    print("some_var is smaller than 10.")
else:                  # 可选
    print("some_var is indeed 10.")
```

    some_var is smaller than 10.



```python
# for item in list
# 迭代取出list中的所有item进行计算
for animal in ["dog", "cat", "mouse"]:
    # You can use format() to interpolate formatted strings
    print("{} is a mammal".format(animal))
```

    dog is a mammal
    cat is a mammal
    mouse is a mammal



```python
# range(n)方法返回一个list,[0,1,2,...,n-1]
for i in range(4):
    print(i)
```

    0
    1
    2
    3



```python
# range(start,end)返回一个list，[start, start+1, ..., end-1]
for i in range(4, 8):
    print(i)
```

    4
    5
    6
    7



```python
# range(start,end,step)返回一个list，[start, start+step, ..., (直到>=end)]
for i in range(4, 8, 2):
    print(i)
```

    4
    6



```python
# while循环，持续迭代知道不满足判断条件
x = 0
while x < 4:
    print(x)
    x += 1  # Shorthand for x = x + 1
```

    0
    1
    2
    3



```python
# 可以通过try except来处理异常（避免报错直接退出）
try:
    # raise方法，可以手动报错
    raise IndexError("This is an index error")
except IndexError as e:
    # pass保留字代表这一行啥不也干
    pass
except (TypeError, NameError):
    # 如果有多个except，可以同时执行
    pass
# 可选，如果try的代码块没有问题，则执行
else:
    print("All good!")
# 可选，不管有没有问题，都会执行finally中的代码块
finally:
    print("We can clean up resources here")
```

    We can clean up resources here



```python
# 通常open(fileName)之后，需要调用close方法来释放内存
# 为了避免代码出错，产生内存垃圾，需要
# try:
#     open
# finally:
#     close
# 也可以通过with open() as name:来进行声明，该声明块结束后会自动close
with open("myfile.txt") as f:
    for line in f:
        print(line)
```


```python
# Python提供一种基础抽象方法叫做Iterable（可迭代的）
# 一个iterable对象，可以被当作sequence对待
# range函数返回的对象其实就是iterable
filled_dict = {"one": 1, "two": 2, "three": 3}
our_iterable = filled_dict.keys()
print(our_iterable)  # => dict_keys(['one', 'two', 'three']). This is an object that implements our Iterable interface.
```

    dict_keys(['one', 'two', 'three'])



```python
# iterable 可迭代，比如放到for循环中
for i in our_iterable:
    print(i)
```

    one
    two
    three



```python
# 但是无法通过index取出其中的数值
# 会报错
our_iterable[0]
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-114-138f56ebc699> in <module>()
          1 # 但是无法通过index取出其中的数值
          2 # 会报错
    ----> 3 our_iterable[0]
    

    TypeError: 'dict_keys' object does not support indexing



```python
# iterable对象可以通过iter方法生成迭代器
our_iterator = iter(our_iterable)
```


```python
our_iterator
```




    <dict_keyiterator at 0x102e49db8>




```python
# 迭代器可以在遍历过程中记录当前状态（位置）
# 我们可以通过next函数取出迭代器中的下一个item
next(our_iterator)  # => "one"
```




    'one'




```python
# 当前迭代的位置会被存储下来
next(our_iterator)  # => "two"
next(our_iterator)  # => "three"
```




    'three'




```python
# 超出迭代范围，就报错
next(our_iterator)
```


    ---------------------------------------------------------------------------

    StopIteration                             Traceback (most recent call last)

    <ipython-input-119-228a51d4a8ec> in <module>()
    ----> 1 next(our_iterator)
    

    StopIteration: 



```python
# 通过list方法把iterable转化为list，就可以访问所有对象了
list(filled_dict.keys())  # => Returns ["one", "two", "three"]
```




    ['one', 'two', 'three']



