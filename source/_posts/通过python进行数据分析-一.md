---
title: 通过python进行数据分析(一)
date: 2019-04-14 22:09:12
tags: [python, 数据分析, 学习笔记]
---

# Introductory examples

## 1.usa.gov data from bit.ly


```python
# 显示当前路径
%pwd
```




    '/Users/imonce/OneDrive/learning/dataAnalyze/pydata-book-master'




```python
# 回到上一层（..）又回到当前文件夹（pydata-book-master）
%cd ../pydata-book-master
```

    /Users/imonce/OneDrive/learning/dataAnalyze/pydata-book-master



```python
# 创建变量并赋值，这里path是数据所在路径
path = 'ch02/usagov_bitly_data2012-03-16-1331923249.txt'
```


```python
# open：打开path路径代表的文件
# open().readline()：读取文件的第一行，并把指针下移一行（再执行一次读取的就是文件的第二行了，以此类推）
open(path).readline()
```




    '{ "a": "Mozilla\\/5.0 (Windows NT 6.1; WOW64) AppleWebKit\\/535.11 (KHTML, like Gecko) Chrome\\/17.0.963.78 Safari\\/535.11", "c": "US", "nk": 1, "tz": "America\\/New_York", "gr": "MA", "g": "A6qOVH", "h": "wfLQtf", "l": "orofrog", "al": "en-US,en;q=0.8", "hh": "1.usa.gov", "r": "http:\\/\\/www.facebook.com\\/l\\/7AQEFzjSi\\/1.usa.gov\\/wfLQtf", "u": "http:\\/\\/www.ncbi.nlm.nih.gov\\/pubmed\\/22415991", "t": 1331923247, "hc": 1331822918, "cy": "Danvers", "ll": [ 42.576698, -70.954903 ] }\n'




```python
# 导入json包
import json
# 创建变量并赋值，这里path是数据所在路径
path = 'ch02/usagov_bitly_data2012-03-16-1331923249.txt'
# json.loads()：以json格式读取数据，读取出来是key：value对，可以像字典一样查询
# for line in open(path)：逐行遍历path文件中的数据
# [json.loads(line) for line in open(path)]：逐行遍历path文件中的数据，通过按照json格式读取，然后每一行的作为一个item组成list（就是外边那个方括号的作用）
records = [json.loads(line) for line in open(path)]
```


```python
# 取出第一个item（第一行读取的内容）看一下
# 这个语句本身没有打印作用，但是在jupyter里边直接放变量会给你打印出来
# 标准写法应该为print(records[0])
records[0]
```




    {'a': 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.78 Safari/535.11',
     'al': 'en-US,en;q=0.8',
     'c': 'US',
     'cy': 'Danvers',
     'g': 'A6qOVH',
     'gr': 'MA',
     'h': 'wfLQtf',
     'hc': 1331822918,
     'hh': '1.usa.gov',
     'l': 'orofrog',
     'll': [42.576698, -70.954903],
     'nk': 1,
     'r': 'http://www.facebook.com/l/7AQEFzjSi/1.usa.gov/wfLQtf',
     't': 1331923247,
     'tz': 'America/New_York',
     'u': 'http://www.ncbi.nlm.nih.gov/pubmed/22415991'}




```python
# 查询第一个item中，key为'a'的value
records[0]['a']
```




    'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.78 Safari/535.11'



### Counting time zones in pure Python


```python
# 如果查询不存在的key的话会报错
records[0]['cc']
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-8-992e1ec28c8d> in <module>()
          1 # 如果查询不存在的key的话会报错
    ----> 2 records[0]['cc']
    

    KeyError: 'cc'



```python
# for rec in records：吧records这个list里边的item逐个取出，每次取出都用rec命名
# [rec['tz'] for rec in records]：把rec中key为‘tz’的value取出来，作为item构建list
# 直接运行会报错，因为有的行里边是没有‘tz’这个key的
time_zones = [rec['tz'] for rec in records]
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-9-abb6a4fa53e3> in <module>()
          2 # [rec['tz'] for rec in records]：把rec中key为‘tz’的value取出来，作为item构建list
          3 # 直接运行会报错，因为有的行里边是没有‘tz’这个key的
    ----> 4 time_zones = [rec['tz'] for rec in records]
    

    <ipython-input-9-abb6a4fa53e3> in <listcomp>(.0)
          2 # [rec['tz'] for rec in records]：把rec中key为‘tz’的value取出来，作为item构建list
          3 # 直接运行会报错，因为有的行里边是没有‘tz’这个key的
    ----> 4 time_zones = [rec['tz'] for rec in records]
    

    KeyError: 'tz'



```python
# 因此这一句在上一句的基础上，增加if 'tz' in rec，意为只把tz的rec中的value构成list
# 因此time_zones的长度小于records
time_zones = [rec['tz'] for rec in records if 'tz' in rec]
```


```python
# 输出两个list的长度看一下
# records中有120个item是没有‘tz’这个key的
print(len(records),len(time_zones))
```

    3560 3440



```python
# 这个函数的参数sequence应该是一个list
# 这个函数的输出是一个dict，其中key是sequence中的item，value是item出现的次数
def get_counts(sequence):
    # 创建空字典counts
    counts = {}
    # 遍历sequence中的item，命名为x
    for x in sequence:
        # 如果x在counts中作为key出现过
        if x in counts:
            # 将当前x对应的value的值+1
            counts[x] += 1
        # counts的key中没有x
        else:
            # 创建x这个key，并将其对应的value设置为1
            counts[x] = 1
    # 返回counts这个字典
    return counts
```


```python
# 从collections这个包里导入defaultdict这个函数
from collections import defaultdict

# 这个函数的参数sequence应该是一个list
# 这个函数的输出是一个dict，其中key是sequence中的item，value是item出现的次数
def get_counts2(sequence):
    # 创建空字典，字典中的value默认为int类型的变量
    # 意义在于，每次插入一个新的key时，对应的value会自动设置为0，不需要先赋值一次
    counts = defaultdict(int) # values will initialize to 0
    # 遍历sequence中的item，命名为x
    for x in sequence:
        # counts的key中有x就直接+1
        # 没有就插入x这个key，（自动初始化value为0），然后+1
        counts[x] += 1
    # 返回counts这个字典
    return counts
```


```python
# 调用刚刚定义的函数，统计一下time_zones这个list中每个时区出现的次数
counts = get_counts(time_zones)
```


```python
# counts是一个dict，因此可以直接通过key查询value的值
# 看看'America/New_York'这个key对应的value时多少
counts['America/New_York']
```




    1251




```python
# counts.items()：把counts这个字典中的key和value成对取出
# [(count, tz) for tz, count in counts.items()]：把键值对以二元组的形式构成list
[(count, tz) for tz, count in counts.items()]
```




    [(1251, 'America/New_York'),
     (191, 'America/Denver'),
     (33, 'America/Sao_Paulo'),
     (16, 'Europe/Warsaw'),
     (521, ''),
     (382, 'America/Los_Angeles'),
     (10, 'Asia/Hong_Kong'),
     (27, 'Europe/Rome'),
     (2, 'Africa/Ceuta'),
     (35, 'Europe/Madrid'),
     (3, 'Asia/Kuala_Lumpur'),
     (1, 'Asia/Nicosia'),
     (74, 'Europe/London'),
     (36, 'Pacific/Honolulu'),
     (400, 'America/Chicago'),
     (2, 'Europe/Malta'),
     (8, 'Europe/Lisbon'),
     (14, 'Europe/Paris'),
     (5, 'Europe/Copenhagen'),
     (1, 'America/Mazatlan'),
     (3, 'Europe/Dublin'),
     (4, 'Europe/Brussels'),
     (12, 'America/Vancouver'),
     (22, 'Europe/Amsterdam'),
     (10, 'Europe/Prague'),
     (14, 'Europe/Stockholm'),
     (5, 'America/Anchorage'),
     (6, 'Asia/Bangkok'),
     (28, 'Europe/Berlin'),
     (25, 'America/Rainy_River'),
     (5, 'Europe/Budapest'),
     (37, 'Asia/Tokyo'),
     (6, 'Europe/Vienna'),
     (20, 'America/Phoenix'),
     (3, 'Asia/Jerusalem'),
     (3, 'Asia/Karachi'),
     (3, 'America/Bogota'),
     (20, 'America/Indianapolis'),
     (9, 'America/Montreal'),
     (9, 'Asia/Calcutta'),
     (1, 'Europe/Skopje'),
     (4, 'Asia/Beirut'),
     (6, 'Australia/NSW'),
     (6, 'Chile/Continental'),
     (4, 'America/Halifax'),
     (6, 'America/Edmonton'),
     (3, 'Europe/Bratislava'),
     (2, 'America/Recife'),
     (3, 'Africa/Cairo'),
     (9, 'Asia/Istanbul'),
     (1, 'Asia/Novosibirsk'),
     (10, 'Europe/Moscow'),
     (1, 'Europe/Sofia'),
     (1, 'Europe/Ljubljana'),
     (15, 'America/Mexico_City'),
     (10, 'Europe/Helsinki'),
     (4, 'Europe/Bucharest'),
     (4, 'Europe/Zurich'),
     (10, 'America/Puerto_Rico'),
     (1, 'America/Monterrey'),
     (6, 'Europe/Athens'),
     (4, 'America/Winnipeg'),
     (2, 'Europe/Riga'),
     (1, 'America/Argentina/Buenos_Aires'),
     (4, 'Asia/Dubai'),
     (10, 'Europe/Oslo'),
     (1, 'Asia/Yekaterinburg'),
     (1, 'Asia/Manila'),
     (1, 'America/Caracas'),
     (1, 'Asia/Riyadh'),
     (1, 'America/Montevideo'),
     (1, 'America/Argentina/Mendoza'),
     (5, 'Asia/Seoul'),
     (1, 'Europe/Uzhgorod'),
     (1, 'Australia/Queensland'),
     (2, 'Europe/Belgrade'),
     (1, 'America/Costa_Rica'),
     (1, 'America/Lima'),
     (1, 'Asia/Pontianak'),
     (2, 'America/Chihuahua'),
     (2, 'Europe/Vilnius'),
     (3, 'America/Managua'),
     (1, 'Africa/Lusaka'),
     (2, 'America/Guayaquil'),
     (3, 'Asia/Harbin'),
     (2, 'Asia/Amman'),
     (1, 'Africa/Johannesburg'),
     (1, 'America/St_Kitts'),
     (11, 'Pacific/Auckland'),
     (1, 'America/Santo_Domingo'),
     (1, 'America/Argentina/Cordoba'),
     (1, 'Asia/Kuching'),
     (1, 'Europe/Volgograd'),
     (1, 'America/La_Paz'),
     (1, 'Africa/Casablanca'),
     (3, 'Asia/Jakarta'),
     (1, 'America/Tegucigalpa')]




```python
# count_dict是待统计的字典，n是要取出n项，默认为10
def top_counts(count_dict, n=10):
    # counts.items()：把counts这个字典中的key和value成对取出
    # [(count, tz) for tz, count in counts.items()]：把键值对以二元组的形式构成list
    value_key_pairs = [(count, tz) for tz, count in count_dict.items()]
    # 调用python中的list自带的sort()方法，默认按照第一维从小到达排序
    value_key_pairs.sort()
    # [-n:]意思为从倒数第n项一直取到最后一项，也就是说返回的是最大的n个
    return value_key_pairs[-n:]
```


```python
# 看看counts中出现最多的时区
top_counts(counts)
```




    [(33, 'America/Sao_Paulo'),
     (35, 'Europe/Madrid'),
     (36, 'Pacific/Honolulu'),
     (37, 'Asia/Tokyo'),
     (74, 'Europe/London'),
     (191, 'America/Denver'),
     (382, 'America/Los_Angeles'),
     (400, 'America/Chicago'),
     (521, ''),
     (1251, 'America/New_York')]




```python
# 其实有现成的包可以用
# 导入collections包中的Counter函数
from collections import Counter
```


```python
# 通过Counter对time_zones这个list进行统计
counts = Counter(time_zones)
```


```python
# 调用Counter对象的方法most_common(n)可以直接调出最多的n项
counts.most_common(10)
```




    [('America/New_York', 1251),
     ('', 521),
     ('America/Chicago', 400),
     ('America/Los_Angeles', 382),
     ('America/Denver', 191),
     ('Europe/London', 74),
     ('Asia/Tokyo', 37),
     ('Pacific/Honolulu', 36),
     ('Europe/Madrid', 35),
     ('America/Sao_Paulo', 33)]
