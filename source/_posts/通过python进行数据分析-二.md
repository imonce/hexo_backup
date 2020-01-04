---
title: 通过python进行数据分析(二)
date: 2019-04-15 21:25:56
tags: [python, 数据分析]
categories: [Python]
---

### Counting time zones with pandas


```python
# %matplotlib主要是在使用jupyter notebook 或者 jupyter qtconsole的时候才会用到
# 具体作用是当你调用matplotlib.pyplot的绘图函数plot()进行绘图的时候，可以直接在你的python console里面生成图像
# 默认是弹出一个绘图窗口
%matplotlib inline
```


```python
# python有多个版本，Python 2.7到Python 3.x，Python提供了__future__模块，把3.x新版本的特性导入到当前版本
# 这一句的意思是除法按照python3来：
# 区别就是python2里边10/3=3，python3里边10/3=3.3333333333333335
from __future__ import division
# 导入numpy中的随机函数randn
from numpy.random import randn
# 到处numpy包，并命名为np
import numpy as np
# 导入os包
import os
# 导入matplotlib.pyplot，并命名为plt，主要用于绘图
import matplotlib.pyplot as plt
# 导入pandas包，并命名为pd
import pandas as pd
# 利用rc方法，plt.rc('figure',figsize=(10,6))全局默认图像大小为10X6
plt.rc('figure', figsize=(10, 6))
# numpy set print options 小数点后4位
np.set_printoptions(precision=4)
```


```python
# 导入json包
import json
# 赋值
path = 'ch02/usagov_bitly_data2012-03-16-1331923249.txt'
# 逐行遍历path文件中的数据，通过按照json格式读取，然后每一行的作为一个item组成list
lines = open(path).readlines()
records = [json.loads(line) for line in lines]
```


```python
# 导入pandas的两个方法
from pandas import DataFrame, Series
# 导入pandas包，并命名为pd
import pandas as pd
# 建立DataFrame对象，把key作为列名，value作为值填到一张表中，没有的键值对会用NaN（空值）填充
# 并自动生成索引，就是左边的0 1 2 3...
frame = DataFrame(records)
# 打印出来看一下（这是一个pandas对象）
print(frame)
```

           _heartbeat_                                                  a  \
    0              NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    1              NaN                             GoogleMaps/RochesterNY   
    2              NaN  Mozilla/4.0 (compatible; MSIE 8.0; Windows NT ...   
    3              NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8)...   
    4              NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    5              NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    6              NaN  Mozilla/5.0 (Windows NT 5.1) AppleWebKit/535.1...   
    7              NaN  Mozilla/5.0 (Windows NT 6.1; rv:2.0.1) Gecko/2...   
    8              NaN  Opera/9.80 (X11; Linux zbov; U; en) Presto/2.1...   
    9              NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    10             NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:10.0.2)...   
    11             NaN  Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.4...   
    12             NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:10.0.2)...   
    13    1.331923e+09                                                NaN   
    14             NaN  Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US...   
    15             NaN  Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1...   
    16             NaN  Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1...   
    17             NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10.5; r...   
    18             NaN                             GoogleMaps/RochesterNY   
    19             NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    20             NaN  Mozilla/5.0 (compatible; MSIE 9.0; Windows NT ...   
    21             NaN  Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.6...   
    22             NaN  Mozilla/4.0 (compatible; MSIE 8.0; Windows NT ...   
    23             NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_3)...   
    24             NaN  Mozilla/5.0 (Windows; U; Windows NT 5.1; es-ES...   
    25             NaN  Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1...   
    26             NaN  Mozilla/5.0 (Windows NT 6.1) AppleWebKit/535.1...   
    27             NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8)...   
    28             NaN  Mozilla/5.0 (iPad; CPU OS 5_0_1 like Mac OS X)...   
    29             NaN  Mozilla/5.0 (iPad; U; CPU OS 3_2 like Mac OS X...   
    ...            ...                                                ...   
    3530           NaN  Mozilla/5.0 (Windows NT 6.0) AppleWebKit/535.1...   
    3531           NaN  Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10_6...   
    3532           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:10.0.2)...   
    3533           NaN  Mozilla/5.0 (iPad; CPU OS 5_1 like Mac OS X) A...   
    3534           NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8)...   
    3535           NaN  Mozilla/5.0 (Windows NT 5.1; rv:10.0.2) Gecko/...   
    3536           NaN  Mozilla/5.0 (BlackBerry; U; BlackBerry 9800; e...   
    3537           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:10.0.2)...   
    3538           NaN  Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Ma...   
    3539           NaN    Mozilla/5.0 (compatible; Fedora Core 3) FC3 KDE   
    3540           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    3541           NaN  Mozilla/5.0 (X11; U; OpenVMS AlphaServer_ES40;...   
    3542           NaN  Mozilla/5.0 (compatible; MSIE 9.0; Windows NT ...   
    3543  1.331927e+09                                                NaN   
    3544           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:5.0.1) ...   
    3545           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64; rv:10.0.2)...   
    3546           NaN  Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Ma...   
    3547           NaN  Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8)...   
    3548           NaN  Mozilla/5.0 (iPhone; CPU iPhone OS 5_1 like Ma...   
    3549           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    3550           NaN  Mozilla/4.0 (compatible; MSIE 8.0; Windows NT ...   
    3551           NaN  Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKi...   
    3552           NaN  Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US...   
    3553           NaN  Mozilla/4.0 (compatible; MSIE 7.0; Windows NT ...   
    3554           NaN  Mozilla/4.0 (compatible; MSIE 7.0; Windows NT ...   
    3555           NaN  Mozilla/4.0 (compatible; MSIE 9.0; Windows NT ...   
    3556           NaN  Mozilla/5.0 (Windows NT 5.1) AppleWebKit/535.1...   
    3557           NaN                             GoogleMaps/RochesterNY   
    3558           NaN                                     GoogleProducer   
    3559           NaN  Mozilla/4.0 (compatible; MSIE 8.0; Windows NT ...   
    
                                              al     c                cy       g  \
    0                             en-US,en;q=0.8    US           Danvers  A6qOVH   
    1                                        NaN    US             Provo  mwszkS   
    2                                      en-US    US        Washington  xxr3Qb   
    3                                      pt-br    BR              Braz  zCaLwp   
    4                             en-US,en;q=0.8    US        Shrewsbury  9b6kNl   
    5                             en-US,en;q=0.8    US        Shrewsbury  axNK8c   
    6        pl-PL,pl;q=0.8,en-US;q=0.6,en;q=0.4    PL             Luban  wcndER   
    7                    bg,en-us;q=0.7,en;q=0.3  None               NaN  wcndER   
    8                                  en-US, en  None               NaN  wcndER   
    9        pt-BR,pt;q=0.8,en-US;q=0.6,en;q=0.4  None               NaN  zCaLwp   
    10                            en-us,en;q=0.5    US           Seattle  vNJS4H   
    11                            en-us,en;q=0.5    US        Washington  wG7OIH   
    12                            en-us,en;q=0.5    US        Alexandria  vNJS4H   
    13                                       NaN   NaN               NaN     NaN   
    14                            en-us,en;q=0.5    US          Marietta  2rOUYc   
    15       zh-TW,zh;q=0.8,en-US;q=0.6,en;q=0.4    HK  Central District  nQvgJp   
    16       zh-TW,zh;q=0.8,en-US;q=0.6,en;q=0.4    HK  Central District   XdUNr   
    17                            en-us,en;q=0.5    US         Buckfield  zH1BFf   
    18                                       NaN    US             Provo  mwszkS   
    19       it-IT,it;q=0.8,en-US;q=0.6,en;q=0.4    IT            Venice  wcndER   
    20                                     es-ES    ES             Alcal  zQ95Hi   
    21                            en-us,en;q=0.5    US     Davidsonville  wcndER   
    22                                     en-us    US         Hockessin  y3ZImz   
    23                                     en-us    US            Lititz  wWiOiD   
    24       es-es,es;q=0.8,en-us;q=0.5,en;q=0.3    ES            Bilbao  wcndER   
    25    en-GB,en;q=0.8,en-US;q=0.6,en-AU;q=0.4    MY      Kuala Lumpur  wcndER   
    26       ro-RO,ro;q=0.8,en-US;q=0.6,en;q=0.4    CY           Nicosia  wcndER   
    27                            en-US,en;q=0.8    BR            SPaulo  zCaLwp   
    28                                     en-us  None               NaN  vNJS4H   
    29                                     en-us  None               NaN  FPX0IM   
    ...                                      ...   ...               ...     ...   
    3530                          en-US,en;q=0.8    US     San Francisco  xVZg4P   
    3531                                   en-US  None               NaN  wcndER   
    3532                          en-us,en;q=0.5    US        Washington  Au3aUS   
    3533                                   en-us    US      Jacksonville  b2UtUJ   
    3534                                   en-us    US            Frisco  vNJS4H   
    3535                                   en-us    US           Houston  zIgLx8   
    3536                          en-US,en;q=0.5  None               NaN  xIcyim   
    3537     es-es,es;q=0.8,en-us;q=0.5,en;q=0.3    HN       Tegucigalpa  zCaLwp   
    3538                                   en-us    US       Los Angeles  qMac9k   
    3539                                     NaN    US          Bellevue  zu2M5o   
    3540                          en-US,en;q=0.8    US            Payson  wcndER   
    3541                                     NaN    US          Bellevue  zu2M5o   
    3542                                   en-us    US         Pittsburg  y3reI1   
    3543                                     NaN   NaN               NaN     NaN   
    3544                          en-us,en;q=0.5    US        Wentzville  vNJS4H   
    3545                          en-us,en;q=0.5    US     Saint Charles  vNJS4H   
    3546                                   en-us    US       Los Angeles  qMac9k   
    3547                                   en-us    US     Silver Spring  y0jYkg   
    3548                                   en-us    US           Mcgehee  y5rMac   
    3549     sv-SE,sv;q=0.8,en-US;q=0.6,en;q=0.4    SE          Sollefte   eH8wu   
    3550                                   en-us    US      Conshohocken  A00b72   
    3551                          en-US,en;q=0.8  None               NaN  wcndER   
    3552                                     NaN    US           Decatur  rqgJuE   
    3553                                   en-us    US        Shrewsbury  9b6kNl   
    3554                                   en-us    US        Shrewsbury  axNK8c   
    3555                                      en    US           Paramus  e5SvKE   
    3556                          en-US,en;q=0.8    US     Oklahoma City  jQLtP4   
    3557                                     NaN    US             Provo  mwszkS   
    3558                                     NaN    US     Mountain View  zjtI4X   
    3559                                   en-US    US           Mc Lean  qxKrTK   
    
           gr       h            hc           hh   kw              l  \
    0      MA  wfLQtf  1.331823e+09    1.usa.gov  NaN        orofrog   
    1      UT  mwszkS  1.308262e+09         j.mp  NaN          bitly   
    2      DC  xxr3Qb  1.331920e+09    1.usa.gov  NaN          bitly   
    3      27  zUtuOu  1.331923e+09    1.usa.gov  NaN       alelex88   
    4      MA  9b6kNl  1.273672e+09       bit.ly  NaN          bitly   
    5      MA  axNK8c  1.273673e+09       bit.ly  NaN          bitly   
    6      77  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    7     NaN  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    8     NaN  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    9     NaN  zUtuOu  1.331923e+09    1.usa.gov  NaN       alelex88   
    10     WA  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    11     DC  A0nRz4  1.331816e+09    1.usa.gov  NaN    darrellissa   
    12     VA  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    13    NaN     NaN           NaN          NaN  NaN            NaN   
    14     GA  2rOUYc  1.255770e+09    1.usa.gov  NaN          bitly   
    15     00  rtrrth  1.317318e+09         j.mp  NaN     walkeryuen   
    16     00  qWkgbq  1.317318e+09         j.mp  NaN     walkeryuen   
    17     ME  x3jOIv  1.331840e+09    1.usa.gov  NaN  andyzieminski   
    18     UT  mwszkS  1.308262e+09    1.usa.gov  NaN          bitly   
    19     20  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    20     51  ytZYWR  1.331671e+09    bitly.com  NaN        jplnews   
    21     MD  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    22     DE  y3ZImz  1.331064e+09    1.usa.gov  NaN          bitly   
    23     PA  wWiOiD  1.330218e+09    1.usa.gov  NaN          bitly   
    24     59  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    25     14  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    26     04  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    27     27  zUtuOu  1.331923e+09    1.usa.gov  NaN       alelex88   
    28    NaN  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    29    NaN  FPX0IL  1.331923e+09    1.usa.gov  NaN   twittershare   
    ...   ...     ...           ...          ...  ...            ...   
    3530   CA  wqUkTo  1.331908e+09  go.nasa.gov  NaN    nasatwitter   
    3531  NaN  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    3532   DC  A9ct6C  1.331926e+09    1.usa.gov  NaN          ncsha   
    3533   FL  ieCdgH  1.301393e+09  go.nasa.gov  NaN    nasatwitter   
    3534   TX  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    3535   TX  yrPaLt  1.331903e+09      aash.to  NaN         aashto   
    3536  NaN  yG1TTf  1.331728e+09  go.nasa.gov  NaN    nasatwitter   
    3537   08  w63FZW  1.331547e+09    1.usa.gov  NaN      bufferapp   
    3538   CA  qds1Ge  1.310474e+09    1.usa.gov  NaN  healthypeople   
    3539   WA  zDhdro  1.331586e+09       bit.ly  NaN       glimtwin   
    3540   UT  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    3541   WA  zDhdro  1.331586e+09    1.usa.gov  NaN       glimtwin   
    3542   CA  y3reI1  1.331926e+09    1.usa.gov  NaN          bitly   
    3543  NaN     NaN           NaN          NaN  NaN            NaN   
    3544   MO  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    3545   IL  u0uD9q  1.319564e+09    1.usa.gov  NaN   o_4us71ccioa   
    3546   CA  qds1Ge  1.310474e+09    1.usa.gov  NaN  healthypeople   
    3547   MD  y0jYkg  1.331852e+09    1.usa.gov  NaN          bitly   
    3548   AR  xANY6O  1.331916e+09    1.usa.gov  NaN    twitterfeed   
    3549   24  7dtjei  1.260316e+09    1.usa.gov  NaN   tweetdeckapi   
    3550   PA  yGSwzn  1.331918e+09    1.usa.gov  NaN        addthis   
    3551  NaN  zkpJBR  1.331923e+09    1.usa.gov  NaN       bnjacobs   
    3552   AL  xcz8vt  1.331227e+09    1.usa.gov  NaN      bootsnall   
    3553   MA  9b6kNl  1.273672e+09       bit.ly  NaN          bitly   
    3554   MA  axNK8c  1.273673e+09       bit.ly  NaN          bitly   
    3555   NJ  fqPSr9  1.301298e+09    1.usa.gov  NaN   tweetdeckapi   
    3556   OK  jQLtP4  1.307530e+09    1.usa.gov  NaN          bitly   
    3557   UT  mwszkS  1.308262e+09         j.mp  NaN          bitly   
    3558   CA  zjtI4X  1.327529e+09    1.usa.gov  NaN          bitly   
    3559   VA  qxKrTK  1.312898e+09    1.usa.gov  NaN          bitly   
    
                                ll   nk  \
    0      [42.576698, -70.954903]  1.0   
    1     [40.218102, -111.613297]  0.0   
    2        [38.9007, -77.043098]  1.0   
    3     [-23.549999, -46.616699]  0.0   
    4      [42.286499, -71.714699]  0.0   
    5      [42.286499, -71.714699]  0.0   
    6         [51.116699, 15.2833]  0.0   
    7                          NaN  0.0   
    8                          NaN  0.0   
    9                          NaN  0.0   
    10      [47.5951, -122.332603]  1.0   
    11     [38.937599, -77.092796]  0.0   
    12     [38.790901, -77.094704]  1.0   
    13                         NaN  NaN   
    14       [33.953201, -84.5177]  1.0   
    15       [22.2833, 114.150002]  1.0   
    16       [22.2833, 114.150002]  1.0   
    17     [44.299702, -70.369797]  0.0   
    18    [40.218102, -111.613297]  0.0   
    19        [45.438599, 12.3267]  0.0   
    20        [37.516701, -5.9833]  0.0   
    21     [38.939201, -76.635002]  0.0   
    22        [39.785, -75.682297]  0.0   
    23       [40.174999, -76.3078]  0.0   
    24            [43.25, -2.9667]  0.0   
    25        [3.1667, 101.699997]  0.0   
    26      [35.166698, 33.366699]  0.0   
    27      [-23.5333, -46.616699]  0.0   
    28                         NaN  0.0   
    29                         NaN  1.0   
    ...                        ...  ...   
    3530    [37.7645, -122.429398]  0.0   
    3531                       NaN  0.0   
    3532   [38.904202, -77.031998]  1.0   
    3533   [30.279301, -81.585098]  1.0   
    3534   [33.149899, -96.855499]  1.0   
    3535   [29.775499, -95.415199]  1.0   
    3536                       NaN  0.0   
    3537        [14.1, -87.216698]  0.0   
    3538  [34.041599, -118.298798]  0.0   
    3539  [47.615398, -122.210297]  0.0   
    3540  [40.014198, -111.738899]  0.0   
    3541  [47.615398, -122.210297]  0.0   
    3542    [38.0051, -121.838699]  0.0   
    3543                       NaN  NaN   
    3544   [38.790001, -90.854897]  1.0   
    3545     [41.9352, -88.290901]  1.0   
    3546  [34.041599, -118.298798]  1.0   
    3547   [39.052101, -77.014999]  1.0   
    3548   [33.628399, -91.356903]  1.0   
    3549    [63.166698, 17.266701]  1.0   
    3550       [40.0798, -75.2855]  0.0   
    3551                       NaN  0.0   
    3552   [34.572701, -86.940598]  0.0   
    3553   [42.286499, -71.714699]  0.0   
    3554   [42.286499, -71.714699]  0.0   
    3555         [40.9445, -74.07]  1.0   
    3556     [35.4715, -97.518997]  0.0   
    3557  [40.218102, -111.613297]  0.0   
    3558  [37.419201, -122.057404]  0.0   
    3559   [38.935799, -77.162102]  0.0   
    
                                                          r             t  \
    0     http://www.facebook.com/l/7AQEFzjSi/1.usa.gov/...  1.331923e+09   
    1                              http://www.AwareMap.com/  1.331923e+09   
    2                                  http://t.co/03elZC4Q  1.331923e+09   
    3                                                direct  1.331923e+09   
    4                   http://www.shrewsbury-ma.gov/selco/  1.331923e+09   
    5                   http://www.shrewsbury-ma.gov/selco/  1.331923e+09   
    6     http://plus.url.google.com/url?sa=z&n=13319232...  1.331923e+09   
    7                              http://www.facebook.com/  1.331923e+09   
    8     http://www.facebook.com/l.php?u=http%3A%2F%2F1...  1.331923e+09   
    9                                  http://t.co/o1Pd0WeV  1.331923e+09   
    10                                               direct  1.331923e+09   
    11                                 http://t.co/ND7SoPyo  1.331923e+09   
    12                                               direct  1.331923e+09   
    13                                                  NaN           NaN   
    14                                               direct  1.331923e+09   
    15    http://forum2.hkgolden.com/view.aspx?type=BW&m...  1.331923e+09   
    16    http://forum2.hkgolden.com/view.aspx?type=BW&m...  1.331923e+09   
    17                                 http://t.co/6Cx4ROLs  1.331923e+09   
    18                             http://www.AwareMap.com/  1.331923e+09   
    19                             http://www.facebook.com/  1.331923e+09   
    20                             http://www.facebook.com/  1.331923e+09   
    21                             http://www.facebook.com/  1.331923e+09   
    22                                               direct  1.331923e+09   
    23    http://www.facebook.com/l.php?u=http%3A%2F%2F1...  1.331923e+09   
    24                             http://www.facebook.com/  1.331923e+09   
    25                             http://www.facebook.com/  1.331923e+09   
    26                 http://www.facebook.com/?ref=tn_tnmn  1.331923e+09   
    27                                               direct  1.331923e+09   
    28                                               direct  1.331923e+09   
    29                                 http://t.co/5xlp0B34  1.331923e+09   
    ...                                                 ...           ...   
    3530  http://www.facebook.com/l.php?u=http%3A%2F%2Fg...  1.331927e+09   
    3531                                             direct  1.331927e+09   
    3532                              http://www.ncsha.org/  1.331927e+09   
    3533                                             direct  1.331927e+09   
    3534                                             direct  1.331927e+09   
    3535                                             direct  1.331927e+09   
    3536                               http://t.co/g1VKE8zS  1.331927e+09   
    3537                               http://t.co/A8TJyibE  1.331927e+09   
    3538                                             direct  1.331927e+09   
    3539                                             direct  1.331927e+09   
    3540  http://www.facebook.com/l.php?u=http%3A%2F%2F1...  1.331927e+09   
    3541                                             direct  1.331927e+09   
    3542  http://www.facebook.com/l.php?u=http%3A%2F%2F1...  1.331927e+09   
    3543                                                NaN           NaN   
    3544                                             direct  1.331927e+09   
    3545                                             direct  1.331927e+09   
    3546                                             direct  1.331927e+09   
    3547                                             direct  1.331927e+09   
    3548  https://twitter.com/fdarecalls/status/18069759...  1.331927e+09   
    3549                                             direct  1.331927e+09   
    3550   http://www.linkedin.com/home?trk=hb_tab_home_top  1.331927e+09   
    3551  http://plus.url.google.com/url?sa=z&n=13319268...  1.331927e+09   
    3552                                             direct  1.331927e+09   
    3553                http://www.shrewsbury-ma.gov/selco/  1.331927e+09   
    3554                http://www.shrewsbury-ma.gov/selco/  1.331927e+09   
    3555                                             direct  1.331927e+09   
    3556  http://www.facebook.com/l.php?u=http%3A%2F%2F1...  1.331927e+09   
    3557                           http://www.AwareMap.com/  1.331927e+09   
    3558                                             direct  1.331927e+09   
    3559                               http://t.co/OEEEvwjU  1.331927e+09   
    
                           tz                                                  u  
    0        America/New_York        http://www.ncbi.nlm.nih.gov/pubmed/22415991  
    1          America/Denver        http://www.monroecounty.gov/etc/911/rss.php  
    2        America/New_York  http://boxer.senate.gov/en/press/releases/0316...  
    3       America/Sao_Paulo            http://apod.nasa.gov/apod/ap120312.html  
    4        America/New_York  http://www.shrewsbury-ma.gov/egov/gallery/1341...  
    5        America/New_York  http://www.shrewsbury-ma.gov/egov/gallery/1341...  
    6           Europe/Warsaw  http://www.nasa.gov/mission_pages/nustar/main/...  
    7                          http://www.nasa.gov/mission_pages/nustar/main/...  
    8                          http://www.nasa.gov/mission_pages/nustar/main/...  
    9                                    http://apod.nasa.gov/apod/ap120312.html  
    10    America/Los_Angeles  https://www.nysdot.gov/rexdesign/design/commun...  
    11       America/New_York  http://oversight.house.gov/wp-content/uploads/...  
    12       America/New_York  https://www.nysdot.gov/rexdesign/design/commun...  
    13                    NaN                                                NaN  
    14       America/New_York               http://toxtown.nlm.nih.gov/index.php  
    15         Asia/Hong_Kong  http://www.ssd.noaa.gov/PS/TROP/TCFP/data/curr...  
    16         Asia/Hong_Kong  http://www.usno.navy.mil/NOOC/nmfc-ph/RSS/jtwc...  
    17       America/New_York  http://www.usda.gov/wps/portal/usda/usdahome?c...  
    18         America/Denver        http://www.monroecounty.gov/etc/911/rss.php  
    19            Europe/Rome  http://www.nasa.gov/mission_pages/nustar/main/...  
    20           Africa/Ceuta  http://voyager.jpl.nasa.gov/imagesvideo/uranus...  
    21       America/New_York  http://www.nasa.gov/mission_pages/nustar/main/...  
    22       America/New_York  http://portal.hud.gov/hudportal/documents/hudd...  
    23       America/New_York  http://www.tricare.mil/mybenefit/ProfileFilter...  
    24          Europe/Madrid  http://www.nasa.gov/mission_pages/nustar/main/...  
    25      Asia/Kuala_Lumpur  http://www.nasa.gov/mission_pages/nustar/main/...  
    26           Asia/Nicosia  http://www.nasa.gov/mission_pages/nustar/main/...  
    27      America/Sao_Paulo            http://apod.nasa.gov/apod/ap120312.html  
    28                         https://www.nysdot.gov/rexdesign/design/commun...  
    29                         http://www.ed.gov/news/media-advisories/us-dep...  
    ...                   ...                                                ...  
    3530  America/Los_Angeles  http://www.nasa.gov/multimedia/imagegallery/im...  
    3531                       http://www.nasa.gov/mission_pages/nustar/main/...  
    3532     America/New_York  http://portal.hud.gov/hudportal/HUD?src=/press...  
    3533     America/New_York                         http://apod.nasa.gov/apod/  
    3534      America/Chicago  https://www.nysdot.gov/rexdesign/design/commun...  
    3535      America/Chicago  http://ntl.bts.gov/lib/44000/44300/44374/FHWA-...  
    3536                       http://www.nasa.gov/mission_pages/hurricanes/a...  
    3537  America/Tegucigalpa            http://apod.nasa.gov/apod/ap120312.html  
    3538  America/Los_Angeles  http://healthypeople.gov/2020/connect/webinars...  
    3539  America/Los_Angeles  http://www.federalreserve.gov/newsevents/press...  
    3540       America/Denver  http://www.nasa.gov/mission_pages/nustar/main/...  
    3541  America/Los_Angeles  http://www.federalreserve.gov/newsevents/press...  
    3542  America/Los_Angeles  http://www.sba.gov/community/blogs/community-b...  
    3543                  NaN                                                NaN  
    3544      America/Chicago  https://www.nysdot.gov/rexdesign/design/commun...  
    3545      America/Chicago  https://www.nysdot.gov/rexdesign/design/commun...  
    3546  America/Los_Angeles  http://healthypeople.gov/2020/connect/webinars...  
    3547     America/New_York  http://www.epa.gov/otaq/regs/fuels/additive/e1...  
    3548      America/Chicago    http://www.fda.gov/Safety/Recalls/ucm296326.htm  
    3549     Europe/Stockholm  http://www.nasa.gov/mission_pages/WISE/main/in...  
    3550     America/New_York  http://www.nlm.nih.gov/medlineplus/news/fullst...  
    3551                       http://www.nasa.gov/mission_pages/nustar/main/...  
    3552      America/Chicago  http://travel.state.gov/passport/passport_5535...  
    3553     America/New_York  http://www.shrewsbury-ma.gov/egov/gallery/1341...  
    3554     America/New_York  http://www.shrewsbury-ma.gov/egov/gallery/1341...  
    3555     America/New_York  http://www.fda.gov/AdvisoryCommittees/Committe...  
    3556      America/Chicago  http://www.okc.gov/PublicNotificationSystem/Fo...  
    3557       America/Denver        http://www.monroecounty.gov/etc/911/rss.php  
    3558  America/Los_Angeles                http://www.ahrq.gov/qual/qitoolkit/  
    3559     America/New_York  http://herndon-va.gov/Content/public_safety/Pu...  
    
    [3560 rows x 18 columns]



```python
# 打印frame这个表中的‘tz’列的前10行
# 注意，这还是一个pandas对象，出了索引，还有Name：tz，dtype（数据类型）：object
print(frame['tz'][:10])
```

    0     America/New_York
    1       America/Denver
    2     America/New_York
    3    America/Sao_Paulo
    4     America/New_York
    5     America/New_York
    6        Europe/Warsaw
    7                     
    8                     
    9                     
    Name: tz, dtype: object



```python
# 统计frame表格中‘tz’列的值的出现次数
tz_counts = frame['tz'].value_counts()
# 输出前10个看看
# 空值出现了521次
print(tz_counts[:10])
```

    America/New_York       1251
                            521
    America/Chicago         400
    America/Los_Angeles     382
    America/Denver          191
    Europe/London            74
    Asia/Tokyo               37
    Pacific/Honolulu         36
    Europe/Madrid            35
    America/Sao_Paulo        33
    Name: tz, dtype: int64



```python
# 空值处理起来会比较麻烦（直接进行操作容易报错）
# fillna（str）方法可以直接用str替代所有的空值
clean_tz = frame['tz'].fillna('Missing')
# 上边替代的不存在key的空值，但是还有另一种空值，key是存在的，但是value是空的（可以看作长度是0的字符串）
# 这里把所有value是‘’的替代成'Unknown'
clean_tz[clean_tz == ''] = 'Unknown'
# 我们再重新统计一遍看看
tz_counts = clean_tz.value_counts()
print(tz_counts[:10])
```

    America/New_York       1251
    Unknown                 521
    America/Chicago         400
    America/Los_Angeles     382
    America/Denver          191
    Missing                 120
    Europe/London            74
    Asia/Tokyo               37
    Pacific/Honolulu         36
    Europe/Madrid            35
    Name: tz, dtype: int64



```python
# 定义画布尺寸为10*4 单位是英寸
plt.figure(figsize=(10, 4))
```




    <matplotlib.figure.Figure at 0x10d7ae940>




    <matplotlib.figure.Figure at 0x10d7ae940>



```python
# 把tz_counts的统计结果绘制成图，类型为柱状图bar，横向h（horizontal）
# kind : str
# ‘line’ : line plot (default)
# ‘bar’ : vertical bar plot
# ‘barh’ : horizontal bar plot
# ‘hist’ : histogram
# ‘box’ : boxplot
# ‘kde’ : Kernel Density Estimation plot
# ‘density’ : same as ‘kde’
# ‘area’ : area plot
# ‘pie’ : pie plot
# ‘scatter’ : scatter plot
# ‘hexbin’ : hexbin plot
# rot : int, default None. Rotation for ticks (xticks for vertical, yticks for horizontal plots)
tz_counts[:10].plot(kind='barh', rot=0)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x10d784eb8>



![](https://raw.githubusercontent.com/imonce/imgs/master/20190415212511.png)



```python
# 看看frame这个表格里‘a’列的第二行（index是1）是什么
print(frame['a'][1])
```

    GoogleMaps/RochesterNY



```python
# 在jupyter里边直接写变量名会自动print出来，是一种不标准的简化写法
frame['a'][50]
```




    'Mozilla/5.0 (Windows NT 5.1; rv:10.0.2) Gecko/20100101 Firefox/10.0.2'




```python
# 在jupyter里边直接写变量名会自动print出来，是一种不标准的简化写法
frame['a'][51]
```




    'Mozilla/5.0 (Linux; U; Android 2.2.2; en-us; LG-P925/V10e Build/FRG83G) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1'




```python
# dropna()函数可以去掉包含有NaN值得item
# frame.a.drapna()就是提取frame表格里列表为a的那一列，去除掉NA值得那些行的值
# x.split(str) 通过指定分隔符str对字符串x进行切片，默认分隔符为空格，x.split(str)[0]意在取切完片的第一个值
# [x.split()[0] for x in frame.a.dropna()]就是提取frame表格里列表为a的那一列，去除掉NA值得那些行的值，并用split进行分割，并且最后保存分割后的第一个值，构成一个list
# Series是Pandas包中的方法，构建Series对象，添加索引
results = Series([x.split()[0] for x in frame.a.dropna()])
# 打印出来看看，后边的乱七八糟的信息已经没有了
results[:5]
```




    0               Mozilla/5.0
    1    GoogleMaps/RochesterNY
    2               Mozilla/4.0
    3               Mozilla/5.0
    4               Mozilla/5.0
    dtype: object




```python
# Pandas对象又可以直接通过value_counts方法做统计了，取出前8个看看
results.value_counts()[:8]
```




    Mozilla/5.0                 2594
    Mozilla/4.0                  601
    GoogleMaps/RochesterNY       121
    Opera/9.80                    34
    TEST_INTERNET_AGENT           24
    GoogleProducer                21
    Mozilla/6.0                    5
    BlackBerry8520/5.0.0.681       4
    dtype: int64




```python
# frame.a.notnull()：frame表格的a列中，不是NaN就是true，是NaN就是False
# frame[frame.a.notnull()]：取出frame这个表格中所有a列不为NaN的行
cframe = frame[frame.a.notnull()]
# 随便打几行看看，a列没有NaN（其他列暂时不管）
cframe[:10]
```

    （表格太大了，略）



```python
# cframe['a'].str 意思是将cframe表格中的‘a’列取出来，转化为padas 中string方法组成的列表，可以调用string相关的方法
# .contains('Windows') string中包含Windows返回True，不包含返回False
# 顺便输出前10行看看
cframe['a'].str.contains('Windows')[:10]
```




    0     True
    1    False
    2     True
    3    False
    4     True
    5     True
    6     True
    7     True
    8    False
    9     True
    Name: a, dtype: bool




```python
# 要理解np.where()这个函数，先看下边几行
# >>> a
# array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
# >>> np.where(a < 5, 0, 1)
# array([ 0,  0,  0,  0,  0, 1, 1, 1, 1, 1])
# 也就是说，对于array a，满足<5的项，用0代替，不满足<5的项用1代替
# np.where(cframe['a'].str.contains('Windows'), 'Windows', 'Not Windows')，返回True的用Windows替代，返回False的用Not Windows替代
operating_system = np.where(cframe['a'].str.contains('Windows'),
                            'Windows', 'Not Windows')
# 打印前五行，因通过numpy方法得到的结果，所以是一个numpy array
operating_system[:5]
```




    array(['Windows', 'Not Windows', 'Windows', 'Not Windows', 'Windows'],
          dtype='<U11')




```python
# cframe.groupby([key1, key2, ...])为将cframe表格中的数据按照key进行分组
# key可以是2种：1. cframe表格中的列名，2.长度和cframe表格相同的数组
# cframe.groupby(['tz', operating_system])的意思就是：
# 第一个分组维度为‘tz’列，改列值相同的为一组
# 第二个分组维度为operating_system数组，也就是Windows的一组，Not Windows的一组
# .size()为统计每个小组的数据量
# .unstack()为把堆叠的分组表格展开（把这个函数去掉试一下就知道区别了）
# .fillna(0)之前说了，把不存在值的格子填充上0
agg_counts = cframe.groupby(['tz', operating_system]).size().unstack().fillna(0)
# 打出前10行看看
# 第一行不是总数，tz的值为空字符串‘’而已
agg_counts[:10]
```




<div><style>    .dataframe thead tr:only-child th {        text-align: right;    }    .dataframe thead th {        text-align: left;    }    .dataframe tbody tr th {        vertical-align: top;    }</style><table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>Not Windows</th><th>Windows</th></tr><tr><th>tz</th><th></th><th></th></tr></thead><tbody><tr><th></th><td>245.0</td><td>276.0</td></tr><tr><th>Africa/Cairo</th><td>0.0</td><td>3.0</td></tr><tr><th>Africa/Casablanca</th><td>0.0</td><td>1.0</td></tr><tr><th>Africa/Ceuta</th><td>0.0</td><td>2.0</td></tr><tr><th>Africa/Johannesburg</th><td>0.0</td><td>1.0</td></tr><tr><th>Africa/Lusaka</th><td>0.0</td><td>1.0</td></tr><tr><th>America/Anchorage</th><td>4.0</td><td>1.0</td></tr><tr><th>America/Argentina/Buenos_Aires</th><td>1.0</td><td>0.0</td></tr><tr><th>America/Argentina/Cordoba</th><td>0.0</td><td>1.0</td></tr><tr><th>America/Argentina/Mendoza</th><td>0.0</td><td>1.0</td></tr></tbody></table></div>




```python
# agg_counts.sum(1) 意思是按照第二维方向相加，也就是把Windows和Not Windows加到一起
# 这里打印10行
agg_counts_sum = agg_counts.sum(1)
agg_counts_sum[:10]
```




    tz
                                      521.0
    Africa/Cairo                        3.0
    Africa/Casablanca                   1.0
    Africa/Ceuta                        2.0
    Africa/Johannesburg                 1.0
    Africa/Lusaka                       1.0
    America/Anchorage                   5.0
    America/Argentina/Buenos_Aires      1.0
    America/Argentina/Cordoba           1.0
    America/Argentina/Mendoza           1.0
    dtype: float64




```python
# .argsort()：按照从小到大的顺序排列，返回排列后item在原数组中对应的index
indexer = agg_counts_sum.argsort()
# 打出前10个，这个时候左边的tz和右边的数字已经没有对应关系了
# 第一个24表示，按照从小到大排序，排在第一位的应该是原本数组中排在第25个（index为24）的那个item
# 第二个20表示，按照从小到大排序，排在第二位的应该是原本数组中排在第21个（index为20）的那个item
# 以此类推
indexer[:10]
```




    tz
                                      24
    Africa/Cairo                      20
    Africa/Casablanca                 21
    Africa/Ceuta                      92
    Africa/Johannesburg               87
    Africa/Lusaka                     53
    America/Anchorage                 54
    America/Argentina/Buenos_Aires    57
    America/Argentina/Cordoba         26
    America/Argentina/Mendoza         55
    dtype: int64




```python
# 让原本的数字按照这个排好的indexer输出，我们就可以得到排序后的数据了
agg_counts_sum[indexer]
```




    tz
    America/Mazatlan                     1.0
    America/La_Paz                       1.0
    America/Lima                         1.0
    Europe/Volgograd                     1.0
    Europe/Sofia                         1.0
    Asia/Manila                          1.0
    Asia/Nicosia                         1.0
    Asia/Riyadh                          1.0
    America/Monterrey                    1.0
    Asia/Novosibirsk                     1.0
    Australia/Queensland                 1.0
    America/Santo_Domingo                1.0
    Asia/Yekaterinburg                   1.0
    America/St_Kitts                     1.0
    America/Tegucigalpa                  1.0
    America/Montevideo                   1.0
    Europe/Ljubljana                     1.0
    Asia/Pontianak                       1.0
    Europe/Uzhgorod                      1.0
    Africa/Casablanca                    1.0
    Africa/Johannesburg                  1.0
    Africa/Lusaka                        1.0
    America/Argentina/Buenos_Aires       1.0
    America/Argentina/Cordoba            1.0
    America/Argentina/Mendoza            1.0
    Europe/Skopje                        1.0
    America/Caracas                      1.0
    America/Costa_Rica                   1.0
    Asia/Kuching                         1.0
    Europe/Riga                          2.0
                                       ...  
    America/Montreal                     9.0
    Asia/Calcutta                        9.0
    America/Puerto_Rico                 10.0
    Asia/Hong_Kong                      10.0
    Europe/Helsinki                     10.0
    Europe/Prague                       10.0
    Europe/Oslo                         10.0
    Europe/Moscow                       10.0
    Pacific/Auckland                    11.0
    America/Vancouver                   12.0
    Europe/Stockholm                    14.0
    Europe/Paris                        14.0
    America/Mexico_City                 15.0
    Europe/Warsaw                       16.0
    America/Phoenix                     20.0
    America/Indianapolis                20.0
    Europe/Amsterdam                    22.0
    America/Rainy_River                 25.0
    Europe/Rome                         27.0
    Europe/Berlin                       28.0
    America/Sao_Paulo                   33.0
    Europe/Madrid                       35.0
    Pacific/Honolulu                    36.0
    Asia/Tokyo                          37.0
    Europe/London                       74.0
    America/Denver                     191.0
    America/Los_Angeles                382.0
    America/Chicago                    400.0
                                       521.0
    America/New_York                  1251.0
    Length: 97, dtype: float64




```python
# agg_counts.take([index1, index2, ...], axis=0)：在axis维度（0为行，1为列）上，按照index顺序取出数据
# 这里没有输入axis的值，默认为0，按行取
# 顺序为按照indexer
# [-10:]：把最后10个取出来
count_subset = agg_counts.take(indexer)[-10:]
count_subset
```




<div><style>    .dataframe thead tr:only-child th {        text-align: right;    }    .dataframe thead th {        text-align: left;    }    .dataframe tbody tr th {        vertical-align: top;    }</style><table border="1" class="dataframe"><thead><tr style="text-align: right;"><th></th><th>Not Windows</th><th>Windows</th></tr><tr><th>tz</th><th></th><th></th></tr></thead><tbody><tr><th>America/Sao_Paulo</th><td>13.0</td><td>20.0</td></tr><tr><th>Europe/Madrid</th><td>16.0</td><td>19.0</td></tr><tr><th>Pacific/Honolulu</th><td>0.0</td><td>36.0</td></tr><tr><th>Asia/Tokyo</th><td>2.0</td><td>35.0</td></tr><tr><th>Europe/London</th><td>43.0</td><td>31.0</td></tr><tr><th>America/Denver</th><td>132.0</td><td>59.0</td></tr><tr><th>America/Los_Angeles</th><td>130.0</td><td>252.0</td></tr><tr><th>America/Chicago</th><td>115.0</td><td>285.0</td></tr><tr><th></th><td>245.0</td><td>276.0</td></tr><tr><th>America/New_York</th><td>339.0</td><td>912.0</td></tr></tbody></table></div>




```python
# 设置一块自定义大小的画布，生成画布对象（这里没写残书就是默认）
plt.figure()
```




    <matplotlib.figure.Figure at 0x112f87e10>




    <matplotlib.figure.Figure at 0x112f87e10>



```python
# 把count_subset这个表格中的数据绘制成图，类型为柱状图bar，横向h（horizontal）
# stacked=True表示多个维度的数据堆叠显示（不明白的话改成False看看就知道了）
count_subset.plot(kind='barh', stacked=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1134d2630>




![](https://raw.githubusercontent.com/imonce/imgs/master/20190416211434.png)



```python
# 设置一块自定义大小的画布，生成画布对象（这里没写残书就是默认）
plt.figure()
```




    <matplotlib.figure.Figure at 0x1136fd2e8>




    <matplotlib.figure.Figure at 0x1136fd2e8>



```python
# count_subset.sum(1)：把count_subset按照第一维的方向相加
# count_subset.div(count_subset.sum(1),axis=0)：按行除以相加的结果
# 相当于把每一行所有数值变成了在当前行占的百分比
normed_subset = count_subset.div(count_subset.sum(1), axis=0)
# stacked=True表示多个维度的数据堆叠显示（不明白的话改成False看看就知道了）
normed_subset.plot(kind='barh', stacked=True)
```




    <matplotlib.axes._subplots.AxesSubplot at 0x1138b6fd0>




![](https://raw.githubusercontent.com/imonce/imgs/master/20190416211504.png)


