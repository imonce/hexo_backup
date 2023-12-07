---
title: >-
  [Reading Notes] Quality Assessment of Business Process Models based on
  Thresholds
date: 2020-01-04 15:41:28
tags: [Quality Assessment, Business Process Model, Service Computing]
categories: [Reading Notes, Service Computing]
---

> **reference:**
> https://www.researchgate.net/profile/Francisco_Ruiz14/publication/220831115_Quality_Assessment_of_Business_Process_Models_Based_on_Thresholds/links/5473100f0cf216f8cfae97f4.pdf
> **author:**
> Laura Sánchez-González, Félix García, Jan Mendling, Francisco Ruiz
> **Institution:**
> Grupo Alarcos, Universidad de Castilla La Mancha
> Humboldt-Universität zu Berlin

# Background and Motivation

Process improvement is recognized as the main benefit of process modelling initiatives. Quality considerations are important when conducting a process modelling project. While the early stage of business process design might not be the most expensive ones, they tend to have the highest impact on the benefits and costs of the implemented business processes. In this context, **quality assurance of the models has become a significant objective.**

# Quality from the perspective of understandability and modifiability

The aim of our empirical research approach is to validate the connections between an extensive set of metrics (*number of nodes, diameter, density, coefficient of connectivity, average gateway degree, maximum gateway degree, separability, sequentiality, depth, gateway mismatch, gateway heterogeneity, cyclicity and concurrency*) and the ease with which business process models can be understood (*understandability*) and modified (*modifiability*).

# null hypothesis: There is no correlation between structural metrics and understandability and modifiability

The structural metrics apparently seem to be closely connected with understandability and modifiability.

1. **For understandability** these include Number of Nodes, Gateway Mismatch, Depth, Coefficient of Connectivity and Sequentiality. 
2. **For modifiability** Gateway Mismatch, Density and Sequentiality showed the best results.

![](https://raw.githubusercontent.com/imonce/imgs/master/20200104162021.png)

# Conclusion

After analyzing which measures are most useful, it is interesting to know what values of these measures indicate poor quality in models. That means, thresholds values could be used as an alarm of detecting low-quality structures in conceptual models.

The strength of the correlation of structural metrics with different quality aspects clearly shows the potential of these metrics to accurately capture aspects closely connected with actual usage.