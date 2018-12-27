---
title: 'latex:使用longtable实现表格跨页'
date: 2018-12-27 15:16:55
tags: [latex, long table, 表格跨页]
---

# 方法

使用包`\usepackage{longtable}`，然后使用`\begin{longtable}...\end{longtable}`代替tabular即可

注意***不能在一个表格上同时使用table和longtable***，比如`\begin{table}\begin{longtable}...\end{longtable}\end{table}`，这会使longtable无法分页。

# 栗子

```
\begin{center}
	\begin{longtable}{p{2.4cm}p{5.4cm}p{3.6cm}}
		\caption{服务模式的语义解释和案例}\\
		\label{revenuepatternexplain}\\
		\hline\noalign{\smallskip}
		名称 & 语义解释 & 案例说明 \\
		\hline
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		名称 & 语义解释 & 案例说明 \\
		\hline
	\end{longtable}
\end{center}
```