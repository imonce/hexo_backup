---
title: >-
  [Reading Notes] How Should I Slice My Network? A Multi-Service Empirical
  Evaluation of Resource Sharing Efficiency
date: 2020-01-03 16:56:40
tags: [evaluation, edge computing, service computing]
categories: [service computing, edge computing]
mathjax: true
---


> **reference:**
> https://e-archivo.uc3m.es/bitstream/handle/10016/27943/how_MOBICOM_2018_ps.pdf?sequence=1
> **author:**
> Cristina Marquez, Marco Gramaglia, Marco Fiore, Alert Banchs, Xavier Costa-Perez
> **Institution:**
> Universidad Carlos III Madrid
> CNR-IEIIT (Institute of Electronics, Computer and Telecommunication Engineering, the National Research Council of Italy)
> NEC Laboratories Europe

# FOR A QUICK GLANCE

## What is network slicing or background

Network slicing has profound implications on resource management, as it entails an inherent trade-off between: 

(i) the need for fully dedicated resources to support service customization, and 

(ii) the dynamic resource sharing among services to increase resource efficiency and cost-effectiveness of the system.

## Why study it or what is the problem

While the technology needed to support this paradigm is well understood from a system standpoint, its implications in terms of efficiency are still unclear. 

## How to do

In this paper, we fill such a gap via an empirical study of resource management efficiency in network slicing.

Building on substantial measurement data collected in an operational mobile network 

(i) we quantify the efficiency gap introduced by non-reconfigurable allocation strategies of different kinds of resources, from radio access to the core of the network, and 

(ii) we quantify the advantages of their dynamic orchestration at different timescales.

## As a result

Our results provide insights on the achievable efficiency of network slicing architectures, their dimensioning, and their interplay with resource management algorithms.

# INTRODUCTION

## Background

Current trends in mobile networks point towards a strong diversification of services, which are characterized by increasingly heterogeneous Key Performance Indicator (KPI) and Quality of Service (QoS) requirements.

current mobile network architectures lack the necessary flexibility to meet the ex- treme requirements imposed by such services.

## Network virtualization and slicing

The agenda for 5G networks is to achieve this mainly via network virtualization, which evolves the traditional hardbox paradigm into a cloudified architecture where the once hardware-based network functions are implemented as software Virtual Network Functions (VNFs) running on a general-purpose telco-cloud. Network virtualization enables the deployment of multiple virtual instances of the complete network, named network slices.

Network slicing strategies. Deeper slices (A to E) reserve resources to services across a wider portion of the end-to-end network architecture, but reduce the space for unconstrained resource sharing.

![](https://raw.githubusercontent.com/imonce/imgs/master/20191231112013.png)

## Network slicing and resource management

When instantiating a slice, an operator needs to allocate sufficient computational and communication resources to its VNFs. In some cases, these resources may be dedicated, be- coming inaccessible to other slices. Alternatively,

## Inherent trade-off

(i) service customization, which favours the deployment of specialized slices with tailored functions for each service and, possibly, dedicated and guaranteed resources; 

(ii) resource management efficiency, which increases by dynamically shar- ing the resources of the common infrastructure among the different services and slices; and, 

(iii) system complexity, resulting from deploying more dynamic resource allocation mechanisms that provide higher efficiency at the cost of em- ploying elaborate operation and maintenance functions.

## Contribution of this paper

From a system standpoint, the technology needed to support the different types of slices is well understood or even already available. 

Our aim is to shed light on the trade-offs between customization, efficiency, and complexity in network slicing, by evaluating the impact of resource allocation dynamics at different network points. Based on our analysis, it is thus possible to determine in which cases the gains in efficiency are worth the sacrifice in customization/isolation and/or the extra complexity. Since resource management efficiency in network slicing highly depends on the traffic patterns of different services supported by the various slices, we build on substantial service-level measurement data collected by a major operator in a production mobile network, and: 

(i) quantify the price paid in efficiency when suitable algorithms for dynamic resource allocation are not available, and the operator has to resort to physical network duplication; 

(ii) evaluate the impact of sharing resources at different
locations of the network, including the cloudified core, the virtualized radio access, or the individual antennas; 

(iii) outline the benefit of dynamic resource allocation
at different timescales, i.e., allowing to reallocate resources across slices with different reconfiguration intervals

# NETWORK SCENARIO AND METRICS

## Network slicing scenario

The operator owning the infrastructure implements slices $s \in S$ , each dedicated to a different subset of services. Every network level $\ell$ is composed by a set $C_{\ell}$ of network nodes.

model the mobile network architecture as a hierarchy composed by a fixed number of levels ( $\ell=1, \ldots, L$ ) ordered from the most distributed ( $\ell=1$ )  to the most centralized ( $\ell=L$ ) 

![](https://raw.githubusercontent.com/imonce/imgs/master/20200103163047.png)

## Slice specifications

Denote a slice specification as:

$$
\mathbb{Z}=(f, w)
$$

involves:

(i) Guaranteed time fraction $f$ : the operator engages to guarantee that the traffic demand of the slice is fully serviced during at least a fraction $f \in[0,1]$ of time.

(ii) Averaging window length w: the operator commitment on fraction f above is intended on discrete-time demands of granularity w, with traffic averaged over the disjoint time windows of duration w.

average load over window k covering a time interval of the same name with duration w:

$$
\bar{o} _ {c,s} (k)=\frac{1}{w}\int_{k}o_{c, s}(t)\mathrm{d}t
$$

the amount of resources allocated to slice s at node c during window k:

$$
r_{c, s}^{\mathbb{Z}}(k)
$$

## Resource allocation to slices

Let $F _ {s, c, n} ^ {w}$ be the Cumulative Distribution Function (CDF)
of the demand for slice s at node c during reconfiguration period k, averaged over windows of length w: then, the minimum $\hat{r} _ {c, s} ^ {\mathbb{Z}} (n)$ 
that satisfies Equation (1) can be computed as $\hat{r} _ {c, s} ^ {\mathbb{Z}} (n)=\left(F_{s, c, n}^{w}\right)^{-1}(f)$ .

$$
\mathbb{R} _ {\ell, \tau} ^ {\mathbb{Z}} = \sum _ {s \in \mathcal{S}} \sum _ {\mathcal{C} \in C _ {\ell}} \sum_{n \in \mathcal{T}} \tau \cdot \hat{r} _ {c, s} ^ {\mathbb{Z}} (n)
$$

![](https://raw.githubusercontent.com/imonce/imgs/master/20200103164643.png)

The above equation represents the total amount of resources needed to meet slice specifications, under the possibility of dynamically re-configuring the allocation with periodicity $\tau$ .

## Multiplexing efficiency

In perfect sharing, the allocated resources correspond to those required when there is no isolation among different services, hence traffic multiplexing is maximum. Formally,

$$
\mathbb{P} _ {\ell, \tau} ^ {\mathbb{Z}} = \sum _ {c \in C_{\ell}} \sum _ {n \in \mathcal{T}} \tau \cdot \hat{r} _ {c} ^ {\mathbb{Z}} (n)
$$

Multiplexing efficiency as the ratio between the resources required with network slicing and those needed under perfect sharing:

$$
\mathbb{B} _ {\ell, \tau} ^ {\mathbb{Z}} = \mathbb{R} _ {\ell, \tau} ^ {\mathbb{Z}} / \mathbb{P} _ {\ell, \tau} ^ {\mathbb{Z}}
$$

# CASE STUDIES

Our two reference urban regions are a large metropolis of
several millions of inhabitants, and a typical medium-sized city with a population of around 500,000, both situated in Europe. Service-level measurement data was collected in the target areas by a major operator with a national market share of around 30%. We leverage these real-world traffic demands to define network slices. Details are in Section 3.1. 

On top of this, we model the hierarchical network infrastructures in the target regions by assuming that the operator deploys level- $\ell$ nodes so as to balance the offered load among them. This is discussed in Section 3.2.

# DATA-DRIVEN EVALUATION

We organise our evaluation as follows. First, we investigate worst-case settings where very stringent slice specifications are enforced, and no dynamic reconfiguration of resources is possible (Section 4.1). We then relax these constraints, and assess efficiency as slice specifications are softened (Section 4.2), or in presence of periodic resource orchestration (Section 4.3). Finally, we evaluate the impact of varied slice configurations (Section 4.4), and of a resource assignment accounting for instantaneous traffic demands (Section 4.5).

# TAKEAWAYS AND PERSPECTIVES

1. Multi-service requires more resources
2. Traffic direction is a factor
3. Loose service level agreements may not help
4. Dynamic resource assignment must also be rapid
5. Aggregating services is beneficial
6. Deployment is slightly more efficient than operation
7. Urban topography has limited impact
8. There is room for improvement
