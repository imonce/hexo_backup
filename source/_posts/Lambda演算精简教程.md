---
title: 'Lambda演算精简教程'
date: 2019-04-24 21:47:53
tags: [lambda, lambda演算, lambda calculus, 学习笔记]
mathjax: true
---

# 前言

这篇文章译自：[https://learnxinyminutes.com/docs/lambda-calculus/](https://learnxinyminutes.com/docs/lambda-calculus/)，写的精简了点，理解起来可能有些困难

建议配合[让我们谈谈 $\lambda$ 演算.pdf](https://github.com/imonce/Lambda-Calculus/blob/master/lambda.pdf)一起食用（出自[https://github.com/txyyss/Lambda-Calculus/releases](https://github.com/txyyss/Lambda-Calculus/releases)）。这篇不算长，深入浅出，写的也极好。

# 整体介绍

Lambda演算( $\lambda$ 演算)由Alonzo Church提出，是世界上最简洁的编程语言。尽管没有数字、字符串、布尔值等非函数数据类型，lambda演算还是可以表达任何图灵机。

Lambda演算由三种元素组成：变量（variables），函数（functions），以及应用（applications）。

|名称|语法|例子|解释|
|:--|:--|:--|:--|
|Variable|<name\>|x|一个名为“x”的变量|
|Function| $\lambda$ <parameters\>.<body\>| $\lambda$ x.x|一个拥有参数“x”以及函数体x的函数|
|Application|<function\><variable or function\>|( $\lambda$ x.x).a|调用函数“ $\lambda$ x.x”且参数值为“a”|

最基础的函数就是恒等函数： $\lambda$ x.x（即f(x)=x）。第一个“x”代表函数的参数，第二个“x”代表函数体。

# 自由变量vs约束变量

- 在 $\lambda$ x.x函数中，x被称为约束变量，因为它同时位于函数体和参数中。
- 在 $\lambda$ x.y函数中，y被称为自由变量，因为它从未被事先声明过。

# 计算

通过 $\beta$ 规约进行计算，其基本上是词法范围的替代。

在计算表达式( $\lambda$ x.x)a时，我们用“a”替换函数体中出现的所有“x”。

- ( $\lambda$ x.x)a 计算结果为：a
- ( $\lambda$ x.y)a 计算结果为：y

也可以创建高阶函数：

- ( $\lambda$ x.( $\lambda$ y.x))a 计算结果为： $\lambda$ y.a

虽然lambda演算传统上只支持单参数函数，但是我们可以使用一种称为currying的技术创建多参数函数。

- ( $\lambda$ x. $\lambda$ y. $\lambda$ z.xyz) 即 f(x, y, z) = ((x y) z)

有时 $\lambda$ xy.<body\>可与 $\lambda$ x. $\lambda$ y.<body\>交替使用。

重要的是要认识到传统的lambda演算没有数字，字符或任何非函数数据类型！

# 布尔逻辑

在lambda演算中没有“True”或“False”。甚至没有1或0。

取而代之的是：

- T表示为： $\lambda$ x. $\lambda$ y.x
- F表示为： $\lambda$ x. $\lambda$ y.y

首先，我们可以定义一个“if”函数 $\lambda$ btf，如果b为True则返回t，如果b为False则返回f

IF 也等同于 $\lambda$ b. $\lambda$ t. $\lambda$ f.b t f

通过使用IF，我们可以定义基础的布尔逻辑运算：

- a AND b 等同于:  $\lambda$ ab.IF a b F
- a OR b 等同于:  $\lambda$ ab.IF a T b
- NOT a 等同于:  $\lambda$ a.IF a F T

注意: IF a b c 本质上是: IF((a b) c)

# 数字

尽管lambda演算中没有数字，我们可以通过邱奇数编码数字。

任意数字n都可以编码为： $n = \lambda f.f^n$ 。因此：

- 0 =  $\lambda$ f. $\lambda$ x.x
- 1 =  $\lambda$ f. $\lambda$ x.f x
- 2 =  $\lambda$ f. $\lambda$ x.f(f x)
- 3 =  $\lambda$ f. $\lambda$ x.f(f(f x))

为了增加邱奇数，我们使用继承函数s(n)=n+1，即

S =  $\lambda$ n. $\lambda$ f. $\lambda$ x.f((n f) x)

通过继承，我们可以定义add：

ADD =  $\lambda$ ab.(a S)b

挑战：试着定义你自己的乘法函数！

# 变得更精致：SKI，SK以及Iota

## SKI组合子演算

使S, K, I，分别为以下函数：

- I x = x
- K x y = x
- S x y z = x z (y z)

我们可以将lambda演算中的表达式转换为SKI组合子演算中的表达式：

1.  $\lambda$ x.x = I
2.  $\lambda$ x.c = Kc
3.  $\lambda$ x.(y z) = S ( $\lambda$ x.y) ( $\lambda$ x.z)

以邱奇数2为例子：

2 =  $\lambda$ f. $\lambda$ x.f(f x)

对于内部部分  $\lambda$ x.f(f x):

$$\begin{split}
\lambda x.f(f x) &=& S ( \lambda x.f) ( \lambda x.(f x))          (case 3) \\\\
&=& S (K f)  (S ( \lambda x.f) ( \lambda x.x))   (case 2, 3) \\\\
&=& S (K f)  (S (K f) I)         (case 2, 1)
\end{split}$$

因此：

$$\begin{split}
2
&=&  \lambda f. \lambda x.f(f x) \\\\
&=&  \lambda f.(S (K f) (S (K f) I)) \\\\
&=&  \lambda f.((S (K f)) (S (K f) I)) \\\\
&=& S ( \lambda f.(S (K f))) ( \lambda f.(S (K f) I)) (case 3)
\end{split}$$

对于第一个参数 \lambda f.(S (K f))：

$$\begin{split}
\lambda f.(S (K f)) 
&=& S ( \lambda f.S) ( \lambda f.(K f))       (case 3)\\\\
&=& S (K S) (S ( \lambda f.K) ( \lambda f.f)) (case 2, 3)\\\\
&=& S (K S) (S (K K) I)       (case 2, 3)
\end{split}$$

对于第二个参数 \lambda f.(S (K f) I)：

$$\begin{split}
 \lambda f.(S (K f) I)
&=&  \lambda f.((S (K f)) I)\\\\
&=& S ( \lambda f.(S (K f))) ( \lambda f.I)             (case 3)\\\\
&=& S (S ( \lambda f.S) ( \lambda f.(K f))) (K I)       (case 2, 3)\\\\
&=& S (S (K S) (S ( \lambda f.K) ( \lambda f.f))) (K I) (case 1, 3)\\\\
&=& S (S (K S) (S (K K) I)) (K I)       (case 1, 2)
\end{split}$$

合到一起：

$$\begin{split}
  2
&=& S ( \lambda f.(S (K f))) ( \lambda f.(S (K f) I))\\\\
&=& S (S (K S) (S (K K) I)) (S (S (K S) (S (K K) I)) (K I))
\end{split}$$

## SK 组合子运算

SKI组合子运算仍可进一步简化。我们可以通过注意I = SKK来移除I组合子。我们可以用SKK替换所有I。

## Iota组合子

SK组合子运算依然不是最简洁的。定义：

$$\begin{split}
ι =  \lambda f.((f S) K)
\end{split}$$

我们就有：

$$\begin{split}
I &=& ιι\\\\
K &=& ι(ιI) = ι(ι(ιι))\\\\
S &=& ι(K) = ι(ι(ι(ιι)))
\end{split}$$

