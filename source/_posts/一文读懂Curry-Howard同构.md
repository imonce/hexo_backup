---
title: 一文读懂Curry-Howard同构
date: 2019-10-10 10:42:29
tags: [Curry-Howard Isomorphism, Type Theory]
mathjax: true
---

# 背景

柯里-霍华德同构，Curry-Howard Isomorphism，又称柯里-霍华德对应（Curry-Howard correspondence）是在计算机程序和数学证明之间的紧密联系；这种对应也叫做公式为类型对应或命题为类型对应。这是对形式逻辑系统和公式计算（computational calculus）之间符号的相似性的推广。命名来自它的两位发现者：美国数学家哈斯凯尔·**柯里**和逻辑学家威廉·阿尔文·**霍瓦德**。

# 同构对应

Curry-Howard 同构显示了推理系统和程序语言之间的相似性，在此框架下：

- 程序语言的语言构造同构为推理系统的推理规则
- 程序的类型同构为逻辑命题
- 闭合程序（不依赖环境的程序）可以同构为一条定理的证明过程，其类型就是一条定理
- 逻辑上下文同构为自由变量类型指派
- Lambda 演算同构为 Gentzen 的自然演绎
	- 函数调用就是蕴含消除
	- 函数抽象就是蕴含介入
	- 参数多态就是全称量化
	- 模板类型就是谓词
	- 结构类型就是合取
	- 联合类型就是析取
	- 收参数但不返回就是否定
	- call/cc 就是双重否定消除
- SK 组合子演算同构为直觉 Hilbert 推理系统
	- S 和 K 就是演算系统的两条公理

# Curry-Howard 同构与 Martin-Löf 类型论系统

这个框架里灵活性最高的是 Martin-Löf 的系统，两个高度抽象的算子—— $\prod$ 和 $\sum$ 进一步泛化了函数调用与合取，这使得它有极其恐怖的抽象能力。这个系统的推理规则是一下几条：

## Introduction rule for $\prod$ 

$$\frac{\Gamma,x:A\vdash b:B}{\Gamma\vdash\lambda x.b:(\prod x:A)B}(\prod I)$$

## Elimination rule for $\prod$ 

$$\frac{\Gamma\vdash f:(\prod x:A)B\quad\Gamma\vdash a:A}{\Gamma\vdash apply(f,a):B[a/x]}(\prod E)$$

Suppose $f = \lambda x.x$, then $apply(f,a)=(\lambda x.x)a$ 

## Introduction rule for $\sum$ 

$$\frac{\Gamma\vdash a:A\quad\Gamma\vdash b:B[a/x]}{\Gamma\vdash\lt a,b\gt:(\sum x:A)B}(\sum I)$$

## Elimination rule for $\sum$ 

$$\frac{\Gamma\vdash c:(\sum x:A)B\quad\Gamma,x:A,y:B\vdash d:C[\lt x,y\gt/z]}{\Gamma\vdash split(c,\lambda x.\lambda y.d):C[c/z]}(\sum E)$$

where: $split(\lt a,b\gt,\lambda x.\lambda y.d)=(\lambda x.\lambda y.d)(a)(b)$ 


> reference
> https://www.zhihu.com/question/22959608/answer/24770830
> https://zh.wikipedia.org/zh-hans/柯里-霍华德同构
> http://www2.math.uu.se/~palmgren/tillog/klogik04-01eng.pdf
