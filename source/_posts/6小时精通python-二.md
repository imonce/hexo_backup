---
title: 'python快速入门：6小时精通python(二)'
date: 2019-04-19 17:35:05
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
## 2. 变量和集合
####################################################
```


```python
# 输出用print()
print("I'm Python. Nice to meet you!")
print("I'm Python. Nice to meet you!")
```

    I'm Python. Nice to meet you!
    I'm Python. Nice to meet you!



```python
# print函数默认在结束时插入换行符
# 可以通过end参数改变
print("Hello, World", end="!")
print("Hello, World", end="!")
```

    Hello, World!Hello, World!


```python
# 在console命令行中获得输入，可以使用input，参数会作为提示进行输出
# Note: 在python早期版本中，input函数名称为raw_input
input_string_var = input("Enter some data: ") 
```

    Enter some data: 123



```python
input_string_var
```




    '123'




```python
# python中没有变量声明，只有赋值
# 变量的命名惯例为小写字母，多个单词通过_连接： lower_case_with_underscores
some_var = 5
some_var  # => 5
```




    5




```python
# 访问一个没有赋值过的变量名，会抛出异常
# 直接看console中的输出来了解异常原因
some_unknown_var  
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-48-17aa7cb5f29d> in <module>()
          1 # 访问一个没有赋值过的变量名，会抛出异常
          2 # 直接看console中的输出来了解异常原因
    ----> 3 some_unknown_var
    

    NameError: name 'some_unknown_var' is not defined



```python
# if 可以用来作为一种表达式 
# a if b else c 意为 b为True取a，b为False取c
hoo = "yahoo!" if 3 > 2 else 2  # => "yahoo!"
hoo
```




    'yahoo!'




```python
# 生成一个空的list
li = []
# 也可以跳过声明直接赋值
other_li = [4, 5, 6]
```


```python
# list有append函数，可以在末尾添加item
li.append(1)    # li is now [1]
li.append(2)    # li is now [1, 2]
li.append(4)    # li is now [1, 2, 4]
li.append(3)    # li is now [1, 2, 4, 3]
# pop函数可以删除list中的最后一个元素
li.pop()        # => 3 and li is now [1, 2, 4]
# 还是把3放回去吧
li.append(3)    # li is now [1, 2, 4, 3] again.
```


```python
# 通过item的index可以访问对应位置item的值
li[0]   # => 1
# 可以通过负数来倒着数，-1代表最后一个
li[-1]  # => 3
```




    3




```python
# 如果index访问的item超出list长度，会抛出异常
li[4]  # IndexError
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-53-9bf3eba2f737> in <module>()
          1 # 如果index访问的item超出list长度，会抛出异常
    ----> 2 li[4]  # Raises an IndexError
    

    IndexError: list index out of range



```python
# li[start:end:step]，你可以通过分片来对list进行部分访问
# li[a:b]，意为取出li中index为a的item至index为b-1的item（含头不含尾）
li[1:3]   # => [2, 4]
# 省略头/尾的参数，则代表 从头开始/到尾结束
li[:3]    # => [1, 2, 4]
li[2:]    # => [4, 3]
# li[a:b:c]意为从li中index为a开始，index每次+c取item，直至所取item的index>=b
li[::2]   # =>[1, 4]
# li[a:b:c]c为负值的时候则倒着取
li[::-1]  # => [3, 4, 2, 1]
```




    [3, 4, 2, 1]




```python
# 如果要对list进行deep copy（复制object所有内容但不是同一对象）
# 使用如下语句
li2 = li[:]
li2 == li # => True
li2 is li # => False
```




    False




```python
# del[index]方法可以删除list中index位置的元素
del li[2]  # li is now [1, 2, 3]
```


```python
# remove(value)方法会删除list中第一个值等于value的item
li.remove(2)
```


```python
# remove方法调用时，如果没有对应value的item，则会报错
li.remove(100)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-58-0f5f01941ba3> in <module>()
          1 # remove方法调用时，如果没有对应value的item，则会报错
    ----> 2 li.remove(100)
    

    ValueError: list.remove(x): x not in list



```python
# insert(index, value)可以在list中的index处插入值为value的item
li.insert(1, 2)  # li is now [1, 2, 3] again
li
```




    [1, 2, 3]




```python
# index(value)方法可以在list中进行查询,返回值为value的item的index
li.index(2)  # => 1
# 没有的话就报错
li.index(4)  # Raises a ValueError as 4 is not in the list
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-60-4520a794cb71> in <module>()
          2 li.index(2)  # => 1
          3 # 没有的话就报错
    ----> 4 li.index(4)  # Raises a ValueError as 4 is not in the list
    

    ValueError: 4 is not in list



```python
# 可以用+直接连接两个list
# 这里没有进行赋值，所以li和other_li都没变
li + other_li  # => [1, 2, 3, 4, 5, 6]
```




    [1, 2, 3, 4, 5, 6]




```python
# 如果调用list内部方法，extend进行连接，则调用方法的list会默认被赋值
li.extend(other_li)
li
```




    [1, 2, 3, 4, 5, 6]




```python
# 通过in关键字，判断value是否存在在list中
1 in li  # => True
```




    True




```python
# len方法可以返回list长度
len(li)  # => 6
```




    6




```python
# Tuple和list相似，但是不可变
tup = (1, 2, 3)
tup[0]      # => 1
tup[0] = 3  # 赋值就报错
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-65-4b7af0c6f896> in <module>()
          2 tup = (1, 2, 3)
          3 tup[0]      # => 1
    ----> 4 tup[0] = 3  # 赋值就报错
    

    TypeError: 'tuple' object does not support item assignment



```python
# 如果长度为1的tuple，需要在唯一的item后添加逗号','来声明自己是tuple
# 否则python会把它的类型解析成唯一item的类型
type((1))   # => <class 'int'>
type((1,))  # => <class 'tuple'>
type(())    # => <class 'tuple'>
```




    tuple




```python
# 大部分list操作都可以应用到tuple上
len(tup)         # => 3
tup + (4, 5, 6)  # => (1, 2, 3, 4, 5, 6)
tup[:2]          # => (1, 2)
2 in tup         # => True
```




    True




```python
# 可以对tuple进行解压，分别赋值给变量
a, b, c = (1, 2, 3)  # a = 1, b = 2 and c = 3
# 还可以进行扩展拆包
a, *b, c = (1, 2, 3, 4)  # a = 1, b = [2, 3] and c = 4
# 如果你不写括号，tuple也会自动生成
d, e, f = 4, 5, 6
# 交换两个变量的值
e, d = d, e  # d is now 5 and e is now 4
```


```python
# Dictionary存储的是key到value的映射
# 生成空的dict
empty_dict = {}
# 也可以直接赋值
filled_dict = {"one": 1, "two": 2, "three": 3}
```


```python
# 可以通过方括号dict[key] = value 查询对应key的值
filled_dict['one']
```




    1




```python
# dictionary中的key必须是不可变类型量（immutable type）
# Immutable types 包括 ints, floats, strings, tuples.
# value是啥都行
invalid_dict = {[1,2,3]: "123"}  # => TypeError: unhashable type: 'list'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-71-b260036dbc7a> in <module>()
          2 # Immutable types 包括 ints, floats, strings, tuples.
          3 # value是啥都行
    ----> 4 invalid_dict = {[1,2,3]: "123"}  # => TypeError: unhashable type: 'list'
    

    TypeError: unhashable type: 'list'



```python
# 通过dictionary中的keys()方法，可以迭代取出字典中的key
# 通过list()可以将该方法的结果转化为list
# python3.7之前的版本，不保证key的取出顺序
# python3.7之后，key会按照在字典中的顺序取出
list(filled_dict.keys())  # => ["three", "two", "one"] in Python <3.7
list(filled_dict.keys())  # => ["one", "two", "three"] in Python 3.7+
```




    ['one', 'two', 'three']




```python
# 同理，通过values方法可以取出values
list(filled_dict.values())  # => [3, 2, 1]  in Python <3.7
list(filled_dict.values())  # => [1, 2, 3] in Python 3.7+
```




    [1, 2, 3, 5]




```python
# 通过in保留字，来检查dictionary中是否包含该key（而非value）
"one" in filled_dict  # => True
1 in filled_dict      # => False
```




    False




```python
# 取一个字典中不存在的key的value会报错
filled_dict["four"]  # KeyError
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-75-6e19dabe2a92> in <module>()
          1 # 取一个字典中不存在的key的value会报错
    ----> 2 filled_dict["four"]  # KeyError
    

    KeyError: 'four'



```python
# 通过get方法，可以避免报错，如果没有，返回None
filled_dict.get("one")      # => 1
filled_dict.get("four")     # => None
# 也可以在get方法中增加第二个参数，来代替查询不到时，默认返回的None
filled_dict.get("one", 4)   # => 1
filled_dict.get("four", 4)  # => 4
```




    4




```python
# setdefault方法可以给不存在的key赋值
# 如果该键值对（key:value）已存在，则不生效
filled_dict.setdefault("five", 5)  # filled_dict["five"] is set to 5
filled_dict.setdefault("five", 6)  # filled_dict["five"] is still 5
```




    5




```python
# 在dictionary中增加键值对，可以使用update方法
filled_dict.update({"four":4})  # => {"one": 1, "two": 2, "three": 3, "four": 4}
# 直接对不存在的key 进行赋值，也可以实现键值对的增加
filled_dict["four"] = 4         # another way to add to dict
```


```python
# 通过del方法可以删除对应key的键值对
del filled_dict["one"]  # Removes the key "one" from filled dict
```


```python
# 在python3.5之后，也可以通过**{}来完成补充扩展操作
{'a': 1, **{'b': 2}}  # => {'a': 1, 'b': 2}
{'a': 1, **{'a': 2}}  # => {'a': 2}
```




    {'a': 2}




```python
# set也是通过{}进行包装的，定义空set时，需要调用set方法
empty_set = set()
# set中的值不能重复（重复值会自动合并）
some_set = {1, 1, 2, 2, 3, 4}  # some_set is now {1, 2, 3, 4}
```


```python
# 和dictionary中的key相似，set的item必须是不可变类型量（也就是list不行）
# set可以看作是一个只有key的dictionary
invalid_set = {[1], 1}  # => Raises a TypeError: unhashable type: 'list'
```


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-85-c6f31d84eee9> in <module>()
          1 # 和dictionary中的key相似，set的item必须是不可变类型量（也就是list不行）
          2 # set可以看作是一个只有key的dictionary
    ----> 3 invalid_set = {[1], 1}  # => Raises a TypeError: unhashable type: 'list'
          4 # tuple就可以
          5 valid_set = {(1,), 1}


    TypeError: unhashable type: 'list'



```python
# 通过add方法向set中添加item
filled_set = some_set
filled_set.add(5)  # filled_set is now {1, 2, 3, 4, 5}
# 重复添加无效
filled_set.add(5)  # it remains as before {1, 2, 3, 4, 5}
```


```python
# 可以通过&运算，来取交集
other_set = {3, 4, 5, 6}
filled_set & other_set  # => {3, 4, 5}
```




    {3, 4, 5}




```python
# 可以通过|取并集
filled_set | other_set  # => {1, 2, 3, 4, 5, 6}
```




    {1, 2, 3, 4, 5, 6}




```python
# 也可以通过-做集合减法（第一个有第二个没有的）
{1, 2, 3, 4} - {2, 3, 5}  # => {1, 4}
```




    {1, 4}




```python
# 可以通过^做对称减法（相当于并集减交集）
{1, 2, 3, 4} ^ {2, 3, 5}  # => {1, 4, 5}
```




    {1, 4, 5}




```python
# 通过大于小于号检查包含关系
{1, 2} >= {1, 2, 3} # => False
{1, 2} <= {1, 2, 3} # => True
```




    True




```python
# 通过in检查set中是否存在该item
2 in filled_set   # => True
10 in filled_set  # => False
```




    False



