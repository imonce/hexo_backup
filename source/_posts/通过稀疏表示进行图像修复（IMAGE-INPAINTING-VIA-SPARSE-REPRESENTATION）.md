---
title: '计算机应用数学学习笔记（三）：通过稀疏表示进行图像修复（IMAGE INPAINTING VIA SPARSE REPRESENTATION）'
date: 2019-06-19 13:05:07
tags: [Image Inpainting, Sparse Representation, 稀疏表示, 图像修复, 计算机应用数学]
categories: [计算机应用数学]
mathjax: true
---

这篇文章提出了一种新的基于冗余字典的图像信号稀疏表示的补丁式图像修复算法，该算法保证低风险的条件下，在处理大的孔的同时，保存图像细节。

与现有的所有工作不同，这篇文章假设每个图像块在冗余字典上允许稀疏表示的假设下，从连续不完全信号恢复的角度考虑图像修复问题。为了保证填充孔与周围环境之间的视觉合理性和一致性约束，这篇文章建议直接从当前图像的完整源区域采样，构造一个冗余信号字典。然后，我们依次计算出孔边界上每个不完整补片的稀疏表示，并将其恢复到整个孔被填满为止。

实验结果表明，该方法能够有效地填充视觉上可信的信息，并降低引入不需要的对象的风险。

# 简介

一般而言，文献中有两种主要的图像修复方法：基于PDE的方法和基于范例的方法。

- 基于PDE的方法目的是将已知区域中的线或边缘延伸到用户指定的区域，这些区域充分注意结构传播，但由于其情况下的模糊效应而不适合处理大区域。
- 基于范例的方法采用纹理合成方法来合成用户指定区域中的像素。

这篇文章主要的贡献是借用信号稀疏表示技术来解决修复问题，并弥合稀疏表示和纹理合成之间的差距。

信号稀疏表示意味着信号允许在冗余字典上进行稀疏表示，我们将在下一节中进行讨论。在这篇文章中，我们将此问题视为不完整图像信号的恢复，每个信号对应一个补丁。他们根据每个补丁的稀疏表示智能地填补漏洞。

# 稀疏表示

Donoho证明了L1范数是L0范数的良好近似。因此，许多技术得到了支持。

Tibshirani提出了一种回归方法：Lasso。他在普通最小二乘回归的损失函数中加入了L1范数罚分，导致系数的稀疏性。

给定字典 $\mathbf{x}=\left[\mathbf{x}^{1}, \mathbf{x}^{2}, \ldots, \mathbf{x}^{\mathbf{N}}\right]$ 以及输入信号 $\mathbf{y}=\left[y_{1}, y_{2}, \dots, y_{p}\right]^{T}$ ，并假定两者都已经进行了正则化。Lasso算法是用来在给定字典情况下估算一个信号的系数 $\beta$ 通过公式：

$\widehat{\boldsymbol{\beta}}=\arg\min\lbrace||\mathbf{y}-\mathbf{x} \boldsymbol{\beta}||_2^2+\lambda||\boldsymbol{\beta}||_1\rbrace$


$||\boldsymbol{\beta}||_{1}$ 引入拟合系数向量的稀疏性，参数 $\lambda$ 控制重构误差与稀疏性之间的权衡。这个公式基于模型 $\mathbf{y}=\mathbf{x} \boldsymbol{\beta}$ 且 $\boldsymbol{\beta}$ 需要是稀疏的。

更有趣的是，当信号的某些组件被破坏时，这意味着模型被修改为

$\mathbf{y}=\mathbf{x} \boldsymbol{\beta}+\mathbf{e}$

其中 $\mathbf{e}$ 代表误差，当且仅当 $y_i$ 是损坏的情况下， $e_i$ 非零。这可以用来帮助我们找到损坏的信号。但是在这篇应用中，需要被修正的区域已经被用户标记，因此我们可以直接判断像素点是否已经被损坏。

我们将损坏的信号的索引集标记为 $I,I=\lbrace i|e_i\neq0\rbrace$ 。 $\boldsymbol{y}\_{|I}$ 表示从 $\boldsymbol{y}$ 中去掉索引在 $I$ 中部分剩下的向量。 $\boldsymbol{x}\_{|I}$ 是对应的词典矩阵，从去掉了 $\boldsymbol{x}$ 中所有索引在 $I$ 中部分剩下的列得到。 现在稀疏系数 $\boldsymbol{\beta}$ 可以通过下边公式来进行计算：

$\widehat{\boldsymbol{\beta}} = \arg \min\lbrace || \mathbf{y}\_{|I}-\mathbf{x}\_{|I}\boldsymbol{\beta}||_2^2+\lambda||\boldsymbol{\beta}||_1\rbrace$

然后我们通过计算出的 $\widehat{\boldsymbol{\beta}}$ 来修复受损的信号：

$\hat{y}_{i}=\left\lbrace\begin{array}{ll}{y_i,} & {\text { if } i \notin I} \\\\ {(\mathbf{x} \hat{\boldsymbol{\beta}})_i,} & {\text { if } i \in I}\end{array}\right.$

# 图像修复算法

## 填补顺序

给定输入图像，用户选择要移除和填充的目标区域。然后通常将丢失的部分视为目标区域。当然，它也可以由用户指定。我们用 $\Omega$ 表示目标区域，用 $\Phi$ 表示源区域，用 $\delta\Omega$ 表示目标区域的边缘。

这篇文章从孔的边界向内部生成图像。这篇工作里按照[[6]](http://vision.csee.wvu.edu/~doretto/courses/2017-fall-cp/reading/ObjectRemovalByExemplar-BasedInpainting_CVPR2003.pdf)来确定填充顺序，因为它有效地保留了结构信息。

在每次迭代过程中，我们计算边缘 $\delta\Omega$ 上的每个像素 $p$ 的优先级 $P(p)$ ，并选择优先级最高的像素设为 $p_m$ 。以 $p_m$ 为中心的补丁将在当前迭代中解决。由于不定的中心在边界上，补丁的一部分像素在目标区域中。因此这个补丁可以被看作是一个不完整的信号，现有部分的信号来自源区域，而缺失的部分来自目标区域。接下来通过稀疏表示来补全这个信号即可。当前不定补全之后再更新边界，进行下一轮迭代即可。

## 信号恢复

现在我们已经定位了像素 $p_m$ 。用一个k维向量 $\Psi_{P_m}$ 表示以 $p_m$ 为中心的补丁。显然 k=nxn ，这里n表示补丁的宽和高。现在根据之前讲的通过稀疏表示，将 $\Psi_{P_m}$ 看作 $\boldsymbol{y}$ ，也就是需要恢复的信号，更加直观地说， $\boldsymbol{y}$ 中属于目标区域的部分的索引集可以被看作  $\boldsymbol{I}$ 。

因此现在，我们可以通过以下两个公式来计算稀疏表示：

$\widehat{\boldsymbol{\beta}} = \arg \min\lbrace ||\Psi_{Pm|I}-\mathbf{x}_{|I} \boldsymbol{\beta}||_2^2+\lambda||\boldsymbol{\beta}||_1\rbrace$

$\hat{\Psi}\_{p_m}^i=\left\lbrace\begin{array}{ll}{\Psi_{p_m}^i,} & {\text { if } i \notin I} \\\\ {(\mathbf{x} \hat{\boldsymbol{\beta}})_i,} & {\text { if } i \in I}\end{array}\right.$

## 字典构建

为了计算信号的稀疏表示，我们首先需要构造一个字典，并在此基础上求解拉索回归。可以采用许多技术来修复字典，例如匹配追踪、基追踪、或K-SVD。

根据我们的观察，填充的目标区域应该在视觉上与源区域一致，这样整个图像看起来似乎是可信的，这意味着不仅纹理应该一致，而且噪音也应该是相同的水平。因此，我们直接对源区域中的所有补丁进行采样，甚至使用它们来构造字典，而不需要进行任何预处理。

例如，如果我们从源区域获得m个补丁，那么固定字典应该有m列。每列对应一个补丁。这种将原始图像作为词典的技术已经应用于人脸识别、背景建模等领域，取得了令人鼓舞的效果。

## 整体算法

![](https://raw.githubusercontent.com/imonce/imgs/master/20190618131653.png)

> reference:
> [https://www.mendeley.com/catalogue/image-inpainting-via-sparse-representation/](https://www.mendeley.com/catalogue/image-inpainting-via-sparse-representation/)