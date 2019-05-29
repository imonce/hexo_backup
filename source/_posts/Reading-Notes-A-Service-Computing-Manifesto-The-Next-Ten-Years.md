---
title: '[Reading Notes] A Service Computing Manifesto: The Next Ten Years'
date: 2019-05-22 14:45:28
tags: [service computing]
category: [reading notes, service computing]
---

# ABSTRACT

这份宣言的主要内容

- 确定阻碍现实世界中服务计算发展和潜在实现的主要障碍
- 提出服务计算的研究方向
- 制定路线图，使服务计算领域能够重新定义自己和 成为社会和经济活动的强大引擎之一。

推荐关注于4个主要研究方向

1. 服务设计 service design
2. 服务集成 service composition
3. 基于众包的信誉 crowdsourcing based reputation
4. 物联网 the Internet of Things

# BACKGROUND

## 服务计算的定义

我们将服务计算（或者称为面向服务的计算）定义为：探索或开发为服务提供广泛支持的计算抽象、计算架构、计算技术和计算工具的学科。

> We define service computing (alternatively termed service- oriented computing) as the discipline that seeks to develop computational abstractions, architectures, techniques, and tools to support services broadly.

## 服务计算的来源

在计算的早期阶段，面临的挑战是以机器可读的格式表示信息，该格式由位和字节组成，称为数据。 随着时间的推移，人们对用意义补充数据产生了浓厚的兴趣，从而将其转化为信息。随着计算技术的进一步发展，人们开始在信息中加入推理，从而产生了知识。最近，对更高抽象级别的需求导致了向知识添加行动，从而产生服务的概念。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190522220858.png)

## 服务计算的目标

服务计算的最终目标是弥合IT和业务服务之间的差距，使IT服务能够更有效地运行业务服务。

> The ultimate goal of service computing is to bridge the gap between IT and business services to en- able IT services to run business services more effectively and efficiently.

服务计算的目标是利用服务范例的功能和简单性及其功能和非功能组件来构建模块化软件应用程序，并为选择和组合服务提供更高级别的抽象，从而将其提升到第一类对象状态。

## 服务计算的相关机构和会议

为了自动化组合服务资源，以根据用户的目标和偏好提供定制的IT服务，标准化机构，如万维网联盟（W3C）和结构化信息标准促进组织（OASIS），已经为实施服务系统领导了规范和标准化工作。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190522221342.png)

## 服务计算的挑战

现有的Web服务标准和技术无法为关键新兴领域的计算需求提供足够的支持，包括移动计算，云计算，大数据和社交计算。这些是[IDC](http://www.idc.com/prodserv/3rd-platform/)命名的第三平台的四项关键技术，目前正在影响全球业务的格局。

## 服务计算和SOA的区别

面向服务的体系结构（SOA）是一种独立于技术的框架，用于定义，注册和调用服务。

服务计算比SOA更广泛，包括对较低级别的服务数据管理和分析的业务流程建模，管理和分析的上层。

SOA已成为服务计算的核心概念，并为实现服务计算提供了基础技术。

## 服务计算和传统计算的区别

1. 服务计算的驱动因素是将服务计算与技术分离，以实现面向服务的系统，充分利用服务计算的承诺和期望
2. 这项工作强调服务计算对计算新兴趋势的贡献和影响

# CHALLENGES IN SERVICE COMPUTING RESEARCH

![](https://raw.githubusercontent.com/imonce/imgs/master/20190522222706.png)

当前的服务计算研究主要集中在七个问题领域：架构，规范语言，协议，框架，生命周期，服务质量，以及跨越自治企业边界建立信任和声誉。

> current service computing research focuses mostly on seven problem areas: architecture, specification languages, protocols, frameworks, lifecycle, quality of service, and the establishment of trust and reputation across the boundaries of autonomous enterprises.

服务计算中经常被忽视的战略挑战是分析为什么服务计算尚未在现实世界中发挥其全部潜力，以及需要采取哪些措施来改变这一点。

一项重大挑战是实现在不同平台上工作的多个组织的无缝合作，以满足消费者的需求。

我们确定了服务计算中的四个新兴研究挑战：服务设计，服务组合，基于群体的声誉和物联网（IoT）。

> We identify four emerging research challenges in service computing: Service Design, Service Composition, Crowdsourcing-Based Reputation, and the Internet of Things (IoT).

## Challenges in Service Design

服务设计是关于对服务性质及其关系的正式理解的映射。

现有设计方法：

1. 通常不会考虑到服务系统固有地将自治部件集合在一起这一事实
2. 没有任何全面的理论框架来定义和分析Web上复杂的服务系统

## Challenges in Service Composition

由于需要对大规模的Web和云服务进行组合，有以下几个挑战：

1. 准确有效地从这些大型存储库中搜索服务正成为一个至关重要的挑战
2. 现有服务选择，组合和推荐方法都是在假设静态数据环境下运行的，这是不充分的
3. 从众多不断变化的设备和服务中选择和组合服务，以实时和上下文感知的方式满足用户需求
4. 基于社会关系的服务构成构成了根本的严峻挑战

## Challenges in Crowdsourcing-Based Reputation

1. 众包的质量。鉴于声誉受到若干相关因素的影响，因此强烈需要预测众包声誉的结果。目前尚不清楚这些因素如何影响众包的质量。
2. 众包贡献者的可信度。一些服务用户的意见可能是不公平的，甚至是对特定服务产品的恶意。
3. 测试平台。对设计适当的评估指标以比较服务的信任和信誉模型存在强烈需求。

## Challenges in the IoT

物联网（IoT）是一个新兴和有前景的领域，它建议将每个有形实体转变为互联网上的一个节点。

物联网提出了两个基本挑战：（1）与事物的沟通（2）事物的管理。

一个挑战是资源有限，传统标准（如SOAP和BPEL）太庞大，无法应用于物联网。 此外，由于架构差异，现有的服务组合模型不能直接用于物联网互操作。与单类型Web服务组件模型（即，服务）相反，IoT组件模型是异构的和多层的（例如，设备，数据，服务和组织）。与传统设置相比，组件的所需功能更具动态性和上下文感知能力。

与服务计算相关的基本物联网挑战包括：

1. 持续维护物联网设备的网络个性和环境。 特别是，物联网事物需要具有反映其物理空间的Web身份和Web表示（例如，Web代理）。 他们还需要在社交，环境，以用户为中心和应用程序环境中进行连接和通信，并且需要维护和管理此类上下文。
2. 不断发现，集成和（重新）使用物联网事物及其数据。 具体而言，物联网环境是一个联合环境，其中事物及其数据，云服务和IT服务（例如，用于数据分析和可视化）通常由具有不同接口的独立提供商以及业务，成本提供。 和QoS模型。 为了提供新的互联网规模的服务，物联网必须（重新）使用他人部署的物联网和其他人为自己的目的收集的数据。

# SERVICE COMPUTING RESEARCH ROADMAP

## Service Design

服务系统的设计应建立在正式的服务模型之上，以便能够有效地访问具有不同功能的大型服务空间。

服务模型支持不同服务及其操作之间的依赖关系非常重要。

总之，满足上述要求的正式服务模型将成为通过服务提供高效透明的计算资源访问的中心，这是充分发挥服务计算潜力的关键一步。

特别是，服务的三个关键特征至关重要：功能，行为和质量。 功能由服务提供的操作指定; 行为反映了如何调用服务操作，并由服务操作之间的依赖性约束决定; 质量决定了服务的非功能特性。

## Service Composition

几个研究方向：

- 大规模的Web和云服务组合。 服务组合研究应扩展到由纯文本描述的非WSDL描述的服务或服务。
- 大数据驱动的服务组合。当前大数据研究的一个重要主题是开发在线处理数据的算法和模型
- 基于社交网络的服务组合。大规模社交网络中的服务选择，推荐和组合应该结合社交网络和复杂的网络分析方法以及信任计算技术。 一个有希望的方向是结合记录服务用户与服务数据的交互的社交网络数据，以检测服务之间的隐藏关系并生成潜在的服务组合。可以通过捕获用户个人判断的社交媒体服务来提取反映用户选择和组合服务的兴趣的特定于域的质量特征

云计算环境为部署服务提供了一个有吸引力的选择，因为它提供了潜在的可扩展性和可访问性。 但是，它引入了与以下问题相关的问题：

1. 维护 - 资源不受服务提供商的明确控制。
2. 安全性 - 云可能不在服务提供商的企业边界内。
3. 服务级别协议（SLA） - 资源分配是云提供商的责任。 例如，服务可能不可用，不仅是由于服务提供商的更新，还因为云提供商的更新。

## Crowdsourcing-Based Reputation

- 众包的质量。应进行社会研究，以调查这些利益因素对众包可靠性的影响以及众包贡献者的范围。这两种因素，例如社会关系和个人偏好，可以同时相互影响。 未来的研究应该针对如何模拟两组因素之间的相互关系以及如何将它们整合起来预测它们对众包数据质量的影响。
- 众包贡献者的可信度。还应探索用于选择具有不同成本和可信赖的众包用户的权衡策略。

## Internet of Things

物联网研究的新方向在于设备发现和集成领域。一个有趣的方向是多跳连接，它利用人与物之间的相互作用来关联物联网事物。

# CONCLUSION

服务计算拥有光明的未来，支持新兴计算领域的巨大进步，如移动计算，云计算，大数据，社交计算等。 我们在本宣言中提出，服务计算的潜力远远大于迄今为止所取得的成就。 我们为将服务计算提升到创新的新高度铺平了道路。 为了开拓进取，我们做出了一个重要的声明，即，要使服务计算范例取得成功，就需要将其与当时的技术分离开来。 挑战可能很困难，但收益很大，没有理由为什么雄心勃勃的研究议程不会给计算机科学和社会带来巨大的好处。