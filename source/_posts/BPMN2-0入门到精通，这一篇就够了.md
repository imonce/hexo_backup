---
title: BPMN2.0入门到精通，这一篇就够了
date: 2019-09-19 10:54:24
tags: [BPMN, BPMN2.0]
---

BPMN2.0入门到精通，这一篇就够了

笔者默认看这篇文章的同学都是了解、知道什么是BPMN的，因此背景知识、历史发展什么的都直接略过，我们直切正题：BPMN中的五个基础元素类别。

1. 流对象（Flow Objects）：流对象是定义业务流程的主要图形元素，主要有三种流对象
	1. 事件（Events）
	2. 活动（Activities）
	3. 网关（Gateways）
2. 数据（Data）：数据主要通过四种元素表示
	1. 数据对象（Data Objects）
	2. 数据输入（Data Inputs）
	3. 数据输出（Data Outputs）
	4. 数据存储（Data Stores）
3. 连接对象（Connecting Objects）：流对象彼此互相连接或者连接到其他信息的方法主要有四种
	1. 顺序流（Sequence Flows）
	2. 信息流（Message Flows）
	3. 协同（Associations）
	4. 数据协同（Data Associations）
4. 泳道（Swimlanes）：有两种方式通过泳道对主要的建模元素进行分组
	1. 泳池：Pools
	2. 泳道：Lanes
5. Artifacts：主要用来提供关于流程的额外信息。BPMN2.0定义两种标准Artifacts，但是建模者或者建模工具可以增加任意多Artifacts。（Artifacts，有的地方翻译成“工件”，但是感觉不管翻译成什么都不够传神，所以本文中就不翻译这个词了。）
	1. 组：Group
	2. 文本注释：Text Annotation

# 流对象（Flow Objects）

## 事件（Event）

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|开始事件 Start|表示一个流程(Process)或一个编排(choreography)的开始|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918171605.png)|
|中间事件 Intermediate|发生在开始和结束事件之间，影响处理流程|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918171742.png)|
|结束事件 End|表示一个流程(Process)或一个编排(choreography)的结束|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918205755.png)|
|其他|开始事件和一些中间事件具有定义事件原因的“触发器”。结束事件可以定义作为序列流路径结束的“结果”。开始事件只能对触发器（“catch”）做出反应。结束事件只能创建（“抛出”）结果。中间事件可以捕获或抛出触发器。对于捕获的事件、触发器，标记未填充；对于抛出的触发器和结果，标记已填充。另外，在bpmn 1.1中用来中断活动的一些事件现在可以在不中断的模式下使用。这些事件的边界是虚线（见右图）。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918210944.png)|

## 活动（Activity）

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|活动 Activity|活动是公司在流程中执行的工作的通用术语。活动可以是原子的或非原子的（聚合物）。作为流程模型一部分的活动类型有：子流程和任务，它们都是圆角矩形。活动用于标准流程Process和编排Choreography。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918212024.png)|
|任务（原子） Task（atomic）|任务是包含在流程中的原子活动。任务是当流程中的工作无法分解为更精细的流程细节级别时使用。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918212345.png)|
|编排任务 Choreography Task|表示一个或多个消息交换的集合。每个编排任务涉及两个参与者。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918213005.png)|
|子流程 Sub-Process|子流程是包含在流程或编排中的复合活动。它是复合的，因为它可以通过一组子活动分解为更细粒度级别的流程或编排。子流程活动主要有以下四类|Collapsed Sub-Process![](https://raw.githubusercontent.com/imonce/imgs/master/20190918213352.png)Expanded Sub-Process![](https://raw.githubusercontent.com/imonce/imgs/master/20190918213453.png)Collapsed Sub- Choreography![](https://raw.githubusercontent.com/imonce/imgs/master/20190918213530.png)Expanded Sub-Choreography![](https://raw.githubusercontent.com/imonce/imgs/master/20190918213637.png)|

## 网关（Gateway）

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|网关 Gateway|网关用于顺序流程和编排中序列流的发散和收敛。因此，它将决定路径的分支、分叉、合并和连接。内部标记将指示行为控制的类型（见下边一行）。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918214000.png)|
|网关控制类型 Gateway Control Type|网关菱形内的图标将指示流控制行为的类型。控制类型包括：•排他型exclusive决策和合并。排他型exclusive和基于事件event-based的网关都执行排他决策，合并排他可以使用或不使用“x”标记来显示。•基于事件event-based和基于并行事件parallel event-based的网关可以启动流程的新实例。•包容型inclusive网关决策和合并。•复杂型complex网关——复杂的条件和情况。•并行parallel网关分叉和连接。每种类型的控件都会影响传入和传出流。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190918214411.png)|

# 数据（Data）

数据对象提供有关需要执行的活动和/或它们产生的内容的信息，数据对象可以表示单个数据对象或数据对象集合。数据输入和数据输出为流程提供相同的信息。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190919100053.png)

# 连接对象（Connecting Objects）

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|顺序流 Sequence Flow|表示活动的执行顺序|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919100734.png)|
|信息流 Message Flow|表示两个参与者之间准备发送和接收的信息流|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919100958.png)|
|协同 Association|协同用于将信息和artifact与图形元素链接。如果有箭头，则表示流向（如数据）。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919101435.png)|

# 泳道（Swimlanes）

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|泳池 Pool|泳池是协作中参与者的图形表示。它还充当一个“泳道”和一个图形容器，用于从其他池中分割一组活动，通常是在B2B环境中。泳池可以具有内部详细信息，以将要执行的进程的形式显示。或者一个泳池可能没有内部细节，也就是说，它可以是一个“黑匣子”。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919102300.png)|
|泳道 Lane|lane是进程中的一个子分区，有时在泳池中，它将垂直或水平地扩展进程的整个长度。泳道用于组织和分类活动。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919102427.png)|

# Artifacts

|元素 Element|描述 Description|符号 Notation|
|:--|:--|:--|
|组 Group|组是同一类别内的图形元素的组。这种类型的分组不影响组内的序列流。类别名称在关系图上显示为组标签。类别可用于文档或分析目的。组是可以在图表上直观显示对象类别的一种方式。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919102731.png)|
|文本注释 Text Annotation|是一个帮助建模者给图形元素增加额外文本说明的机制。|![](https://raw.githubusercontent.com/imonce/imgs/master/20190919102837.png)|

# 总结

至此，你已经通过 **20%** 的时间了解了BPMN2.0 接近 **80%** 的内容。虽然BPMN底层语法以及结构还没有学习，但是这并不影响你已经可以通过BPMN2.0对你所在的业务进行详尽的描述！