---
title: '统计学三大相关性系数:Pearson,Spearman,Kendall'
date: 2020-08-04 10:25:52
tags: [统计学, 相关性分析]
categories: [统计学]
mathjax: true
---

# Pearson

要理解Pearson相关系数，首先要理解协方差（Covariance），协方差是一个反映两个随机变量相关程度的指标，如果一个变量跟随着另一个变量同时变大或者变小，那么这两个变量的协方差就是正值，反之相反，公式如下： 

$$
\operatorname{cov}(x, y)=\frac{\sum_{i=1}^{n}\left(x_{i}-x_{\mu}\right)\left(y_{i}-y_{\mu}\right)}{n-1}
$$

虽然协方差能反映两个随机变量的相关程度（协方差大于0的时候表示两者正相关，小于0的时候表示两者负相关），但是协方差值的大小并不能很好地度量两个随机变量的关联程度。

为了更好的度量两个随机变量的相关程度，引入了Pearson相关系数，其在协方差的基础上除以了两个随机变量的标准：

$$
\delta^{2}=\frac{\sum_{i=1}^{n}\left(x_{i}-x_{\mu}\right)}{n}
$$

容易得出，pearson是一个介于-1和1之间的值，当两个变量的线性关系增强时，相关系数趋于1或-1；当一个变量增大，另一个变量也增大时，表明它们之间是正相关的，相关系数大于0；如果一个变量增大，另一个变量却减小，表明它们之间是负相关的，相关系数小于0；如果相关系数等于0，表明它们之间不存在线性相关关系。

$$
p_{x, y}=\operatorname{cor}(x, y)=\frac{\operatorname{cov}(x, y)}{\delta x \delta y}=\frac{E\left[\left(x-x_{\mu}\right)\left(y-y_{\mu}\right)\right]}{\delta x \delta y}
$$

# Spearman

斯皮尔曼相关性系数，通常也叫斯皮尔曼秩相关系数。“秩”，可以理解成就是一种顺序或者排序，那么它就是根据原始数据的排序位置进行求解，这种表征形式就没有了求皮尔森相关性系数时那些限制。下面来看一下它的计算公式：

$$
\rho_{s}=1-\frac{6 \sum d_{i}^{2}}{n\left(n^{2}-1\right)}
$$

计算过程就是：
对两个变量（X，Y）的数据进行排序（统一用升序或降序），每个变量在排序之后的位置即为其秩次（X', Y'），原始位置相同的X，Y的秩次X', Y'的差值即为 $d_i$ 。n是变量的个数（或者对数）。

# Kendall

Kendall相关系数是对于定类变量的统计，之前讲pearson是对定距变量的统计，而spearman是对定序变量的统计。

Kendall相关系数的计算公式：

$$
Kendall1=\frac{C-D}{\frac{1}{2} N\left(N-1\right)}
$$

另一个计算公式：

$$
Kendall2=\frac{C-D}{\sqrt{\left(N_{3}-N_{1}\right)\left(N_{3}-N_{2}\right)}}
$$

第一个和第二个公式的区别在于，当两变量任何一个中都不存在相同元素时用公式1，两变量中任何一个中存在相同元素用2。

其中N为样本数量。再说N3，N2，N1。

$$
\mathrm{N} 3=\frac{1}{2} N(N-1)
$$

N2、N1就比较复杂，它们各指向一个变量，但是计算方法一致：

$$
\mathrm{N} 2=\sum_{i=1}^{s} \frac{1}{2} v_{i}\left(v_{j}-1\right)
$$

s是指该变量中拥有相同元素的小集合的个数，v就是每个集合中元素的个数。

> Reference:
> https://blog.csdn.net/ichuzhen/article/details/79535226
> https://blog.csdn.net/zmqsdu9001/article/details/82840332?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight
> https://www.jianshu.com/p/93fd5ab408ae