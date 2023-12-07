---
title: Python入门：pickle模块简介
date: 2017-04-24 02:45:19
tags: [Python, Pickle, Python入门]
categories: [Python]
comments: true
---

## 概要
这篇文章介绍了何为pickle以及pickle模块的简单使用方法，即如何使用pickle进行存储存储以及数据的提取，关于pickle模块的其他更加详细的介绍可以参看[https://docs.python.org/2/library/pickle.html](https://docs.python.org/2/library/pickle.html)

## pickle简介

pickle模块是python中用来持久化对象的一个模块。所谓对对象进行持久化，即将对象的数据类型、存储结构、存储内容等所有信息作为文件保存下来以便下次使用。

就比如说你通过pickle将一个数组保存成了文件，那么当你下次通过pickle将这个文件读取出来的时候，你读取到的依然是一个数组，而不是一个看起来长得像数组的字符串。

## 用pickle保存对象到文件

```python
#导入pickle模块
import pickle

#创建一个名为data1的对象
data1 = {'a': '123', 'b': [1, 2, 3, 4]}

#打开(或创建)一个名为data1.pkl的文件，打开方式为二进制写入(参数‘wb’)
file_to_save = open("data1.pkl", "wb")

#通过pickle模块中的dump函数将data1保存到data1.pkl文件中。
#第一个参数是要保存的对象名
#第二个参数是写入到的类文件对象file。file必须有write()接口， file可以是一个以'w'方式打开的文件或者一个StringIO对象或者其他任何实现write()接口的对象。如果protocol>=1，文件对象需要是二进制模式打开的。
#第三个参数为序列化使用的协议版本，0：ASCII协议，所序列化的对象使用可打印的ASCII码表示；1：老式的二进制协议；2：2.3版本引入的新二进制协议，较以前的更高效；-1：使用当前版本支持的最高协议。其中协议0和1兼容老版本的python。protocol默认值为0。
pickle.dump(data1, file_to_save, -1)

#关闭文件对象
file_to_save.close()
```

## 用pickle从文件中读取对象

(请接着上一个脚本运行)
```python
#导入pickle模块
import pickle

#打开一个名为data1.pkl的文件，打开方式为二进制读取(参数‘rb’)
file_to_read = open('data1.pkl', 'rb')

#通过pickle的load函数读取data1.pkl中的对象，并赋值给data2
data2 = pickle.load(file_to_read)

#打印data2
print data2

#关闭文件对象
file_to_read.close()
```



