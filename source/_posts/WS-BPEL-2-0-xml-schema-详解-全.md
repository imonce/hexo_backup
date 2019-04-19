---
title: WS-BPEL 2.0 xml schema 详解(1+2+3全，方便检索)
date: 2019-03-22 16:24:33
tags: [bpel, wsbpel, ws-bpel, bpel2.0, schema, xsd]
---

这篇文章将一行一行的解读wsbpel2.0的源码。

相关xsd语法问题，请参见[XSD学习笔记完整版](https://imonce.github.io/2019/03/15/XSD学习笔记完整版/#简单类型)

wsbpel2.0源码：[ws-bpel_executable.xsd](http://docs.oasis-open.org/wsbpel/2.0/OS/process/executable/ws-bpel_executable.xsd)

# schema声明

```
<xsd:schema xmlns="http://docs.oasis-open.org/wsbpel/2.0/process/executable" 
	xmlns:xsd="http://www.w3.org/2001/XMLSchema" targetNamespace="http://docs.oasis-open.org/wsbpel/2.0/process/executable" elementFormDefault="qualified" blockDefault="#all">
	...
<\xsd>
```

声明命名空间`http://docs.oasis-open.org/wsbpel/2.0/process/executable`，没有前缀。

引入`http://www.w3.org/2001/XMLSchema`的语素并以xsd为前缀。

`elementFormDefault="qualified"`表示所有元素都必须加上前缀以表明其命名空间。

`blockDefault="#all"`表示默认情况下不能通过派生类代替原类型。

# annotation

```
<xsd:annotation>
	<xsd:documentation>
Schema for Executable Process for WS-BPEL 2.0 OASIS Standard 11th April, 2007
	</xsd:documentation>
</xsd:annotation>
```

赠送了一个简单的文档说明。

# import

```
<xsd:import namespace="http://www.w3.org/XML/1998/namespace" schemaLocation="http://www.w3.org/2001/xml.xsd"/>
```

引入`http://www.w3.org/2001/xml.xsd`的xml语素，前缀默认为xml。

# element：process

```
<xsd:element name="process" type="tProcess">
	<xsd:annotation>
		<xsd:documentation>
This is the root element for a WS-BPEL 2.0 process.
		</xsd:documentation>
	</xsd:annotation>
</xsd:element>
```

BPEL的根元素，此处没有定义任何内容，内部元素属性通过type="tProcess"引入。

# complexType：tProcess

```
<xsd:complexType name="tProcess">
	<xsd:complexContent>
		<xsd:extension base="tExtensibleElements">
			<xsd:sequence>
				<xsd:element ref="extensions" minOccurs="0"/>
				<xsd:element ref="import" minOccurs="0" maxOccurs="unbounded"/>
				<xsd:element ref="partnerLinks" minOccurs="0"/>
				<xsd:element ref="messageExchanges" minOccurs="0"/>
				<xsd:element ref="variables" minOccurs="0"/>
				<xsd:element ref="correlationSets" minOccurs="0"/>
				<xsd:element ref="faultHandlers" minOccurs="0"/>
				<xsd:element ref="eventHandlers" minOccurs="0"/>
				<xsd:group ref="activity" minOccurs="1"/>
			</xsd:sequence>
			<xsd:attribute name="name" type="xsd:NCName" use="required"/>
			<xsd:attribute name="targetNamespace" type="xsd:anyURI" use="required"/>
			<xsd:attribute name="queryLanguage" type="xsd:anyURI" default="urn:oasis:names:tc:wsbpel:2.0:sublang:xpath1.0"/>
			<xsd:attribute name="expressionLanguage" type="xsd:anyURI" default="urn:oasis:names:tc:wsbpel:2.0:sublang:xpath1.0"/>
			<xsd:attribute name="suppressJoinFailure" type="tBoolean" default="no"/>
			<xsd:attribute name="exitOnStandardFault" type="tBoolean" default="no"/>
		</xsd:extension>
	</xsd:complexContent>
</xsd:complexType>
```

首先是一个名为tExtensibleElements的扩展，先放一下往后看。

一个sequence，包括extensions，import，partnerLinks，messageExchanges，variables，correlationSets，faultHandlers，eventHandlers，还有一个activity的group，这里全部是ref，我们知道大概有些啥就行了，后边肯定会有详细的定义，先往后看吧。

接着是一堆attribute，包括process的

- 名称name
- 目标命名空间targetNamespace
- 查询语言queryLanguage，默认是urn:oasis:names:tc:wsbpel:2.0:sublang:xpath1.0
- 表达语言expressionLanguage，默认是：urn:oasis:names:tc:wsbpel:2.0:sublang:xpath1.0
- 抑制链接失败suppressJoinFailure，默认是否
- 标准错误退出exitOnStandardFault，默认是否

# complexType：tExtensibleElement

```
<xsd:complexType name="tExtensibleElements">
    <xsd:annotation>
        <xsd:documentation>
This type is extended by other component types to allow elements and attributes from other namespaces to be added at the modeled places.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:sequence>
        <xsd:element ref="documentation" minOccurs="0" maxOccurs="unbounded"/>
        <xsd:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:anyAttribute namespace="##other" processContents="lax"/>
</xsd:complexType>
```

tExtensibleElements这个扩展马上就来了，可以看到，扩展除了0至多个documentation（后边再讲），还有

- element：来自该元素的父元素的目标命名空间之外的任何命名空间的元素，且即使不能获取该命名空间架构，也不会发生任何错误。
- attribute：同上

# element：documentation

```
<xsd:element name="documentation" type="tDocumentation"/>

<xsd:complexType name="tDocumentation" mixed="true">
    <xsd:sequence>
        <xsd:any processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:attribute name="source" type="xsd:anyURI"/>
    <xsd:attribute ref="xml:lang"/>
</xsd:complexType>
```

documentation中：

- element：一个字符元素可混合出现的，元素可随意引入，不在此命名空间也没关系
- attribute：
	- source：通过URI表明来源
	- xml:lang: 文档语言，如en、CN等

# group：activity

```
<xsd:group name="activity">
    <xsd:annotation>
        <xsd:documentation>
All standard WS-BPEL 2.0 activities in alphabetical order. Basic activities and structured activities. Addtional constraints: - rethrow activity can be used ONLY within a fault handler (i.e. "catch" and "catchAll" element) - compensate or compensateScope activity can be used ONLY within a fault handler, a compensation handler or a termination handler
        </xsd:documentation>
    </xsd:annotation>
    <xsd:choice>
        <xsd:element ref="assign"/>
        <xsd:element ref="compensate"/>
        <xsd:element ref="compensateScope"/>
        <xsd:element ref="empty"/>
        <xsd:element ref="exit"/>
        <xsd:element ref="extensionActivity"/>
        <xsd:element ref="flow"/>
        <xsd:element ref="forEach"/>
        <xsd:element ref="if"/>
        <xsd:element ref="invoke"/>
        <xsd:element ref="pick"/>
        <xsd:element ref="receive"/>
        <xsd:element ref="repeatUntil"/>
        <xsd:element ref="reply"/>
        <xsd:element ref="rethrow"/>
        <xsd:element ref="scope"/>
        <xsd:element ref="sequence"/>
        <xsd:element ref="throw"/>
        <xsd:element ref="validate"/>
        <xsd:element ref="wait"/>
        <xsd:element ref="while"/>
    </xsd:choice>
</xsd:group>
```

定义了一个activity的group，用于在其他地方引用，比如说通过tProcess引用到process里边。

一个activity可以是以下元素中的一个，没写到的看后边源码解读好了：

|活动名称|释义|
|:--|:--|
|assign|活动的作用是用新的数据来更新变量的值。Assign活动可以包括任意数量的基本复制操作。|
|compensate|通过该活动做一些补偿动作，通常需要和scope联合使用。只能从故障处理程序或另一个补偿处理活动中调用这个活动。补偿处理程序只能被调用一次。|
|compensateScope||
|empty|无所事事，比如在一个错误发生后可以不做反应来消除这个错误|
|exit|该活动用于立刻终止业务流程实例。所有当前运行的活动必须被立刻终止。不用引用任何终点处理、错误处理或者补偿行为。|
|extensionActivity||
|flow|可以描述更为复杂的活动执行顺序。我们可以利用flow指定一个或多个并行执行的活动。为了定义任意的控制结构，可以在并行的活动中使用链接。|
|forEach||
|if||
|invoke|活动允许业务流程同步或异步调用由合作伙伴提供的服务，服务实现可以是单向或请求-响应操作。Invoke活动使用“partnerLink”来引用伙伴服务。同过“portType”和“operation”指定相应的WSDL接口和操作。|
|pick|活动会等待一组相互排斥事件中的一个事件的发生，然后执行与发生的事件相关联的活动。它会阻塞业务流程执行，以等待某一特定的事件发生，比如接收到一个合适的消息或超时警报响起。当其中任何一个事件被触发后，业务流程就会继续执行，pick也随即完成了，不会再等待其他事件的发生。|
|receive|活动从流程的外部伙伴那获取数据，并将其保存到流程变量。通常一个Receive是一个流程的初始点，它会阻塞执行直到匹配的消息的到达。|
|repeatUntil||
|reply|活动发送消息给伙伴来应答通过receive活动所接收到的消息。receive和reply的组合对应着WSDL portType上定义的一个请求-响应操作。如果receive活动对应着一个单向(one-way)操作，则不能在流程中定义对应的reply活动。|
|rethrow||
|scope|使用这个结构可以将一组活动组织在一起作为一个处理单位。通过这个组织方法多个活动可以使用同一个故障处理、事故处理和补偿处理。通过补偿处理BPEL可以处理长时间的处理。|
|sequence|定义一组按顺序先后执行的活动。执行顺序是sequence活动中嵌套活动的先后顺序。当sequence中的最后一个活动完成后，该sequence活动也就完成了。|
|throw|提示一个错误，一个故障处理可以处理这样的错误。假如一个错误不被处理的话它最终到达最高层后导致过程的终止|
|validate||
|wait|活动会暂停流程执行，等待一段给定的时间或等到某一时刻才继续运行。在WebSphere Process Server 6.0中，开发者可以非常灵活地指定wait中的到期条件，比如等待多少秒，等到特定的一个日期，或是使用内置的日期表现法。也可以使用Java代码来动态指定等待时间。|
|while|继承于传统的结构化编程思想，提供了while-do循环结构的支持。它可以包含一个或多个活动。它指定反复执行其内部活动，直到成功条件不被满足为止。在WPS中允许其使用Java代码来描述条件表达式。|

# element：extensions

```
<xsd:element name="extensions" type="tExtensions"/>

<xsd:complexType name="tExtensions">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="extension" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="extension" type="tExtension"/>

<xsd:complexType name="tExtension">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="namespace" type="xsd:anyURI" use="required"/>
            <xsd:attribute name="mustUnderstand" type="tBoolean" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

一个extensions由1到多个extension组成。

extension扩展自tExtensibleElements，增加了namespace和mustUnderstand两个属性。

# element：import

```
<xsd:element name="import" type="tImport"/>

<xsd:complexType name="tImport">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="namespace" type="xsd:anyURI" use="optional"/>
            <xsd:attribute name="location" type="xsd:anyURI" use="optional"/>
            <xsd:attribute name="importType" type="xsd:anyURI" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

BPEL允许import，import元素在tExtensibleElements的基础上，增加namespace、location、importType三个属性，和xsd的import类似。

# element：partnerLinks

```
<xsd:element name="partnerLinks" type="tPartnerLinks"/>

<xsd:complexType name="tPartnerLinks">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="partnerLink" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="partnerLink" type="tPartnerLink"/>

<xsd:complexType name="tPartnerLink">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="name" type="xsd:NCName" use="required"/>
            <xsd:attribute name="partnerLinkType" type="xsd:QName" use="required"/>
            <xsd:attribute name="myRole" type="xsd:NCName"/>
            <xsd:attribute name="partnerRole" type="xsd:NCName"/>
            <xsd:attribute name="initializePartnerRole" type="tBoolean"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

和extensions相似，partnerLinks由1到多个partnerLink组成，同时支持tExtensibleElements扩展。

partnerLink在tExtensibleElements的基础上，增加了以下5个属性

- name
- partnerLinkType
- myRole
- partnerRole
- initializaPartnerRole

# element：messageExchanges

```
<xsd:element name="messageExchanges" type="tMessageExchanges"/>

<xsd:complexType name="tMessageExchanges">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="messageExchange" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="messageExchange" type="tMessageExchange"/>

<xsd:complexType name="tMessageExchange">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="name" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

和extensions相似，messageExchanges由1到多个messageExchange组成，同时支持tExtensibleElements扩展。

messageExchange在tExtensibleElements的基础上，增加了一个属性

- name

# element：variables

```
<xsd:element name="variables" type="tVariables"/>

<xsd:complexType name="tVariables">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="variable" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="variable" type="tVariable"/>

<xsd:complexType name="tVariable">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="from" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="name" type="BPELVariableName" use="required"/>
            <xsd:attribute name="messageType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="type" type="xsd:QName" use="optional"/>
            <xsd:attribute name="element" type="xsd:QName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

和extensions相似，variables由1到多个variable组成，同时支持tExtensibleElements扩展。

variable在tExtensibleElements的基础上，增加了一个元素

- from

增加了四个属性

- name
- messageType
- type
- element

# simpleType：BPELVariableName

```
<xsd:simpleType name="BPELVariableName">
    <xsd:restriction base="xsd:NCName">
        <xsd:pattern value="[^\.]+"/>
    </xsd:restriction>
</xsd:simpleType>
```

bpel变量的一个限制，BPELVariableName需要满足xsd:NCName限制，不能以`.`开头，且长度大于等于一个字符。

# element：correlationSets

```
<xsd:element name="correlationSets" type="tCorrelationSets"/>

<xsd:complexType name="tCorrelationSets">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="correlationSet" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="correlationSet" type="tCorrelationSet"/>

<xsd:complexType name="tCorrelationSet">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="properties" type="QNames" use="required"/>
            <xsd:attribute name="name" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

和extensions相似，correlationSets由1到多个correlationSet组成，同时支持tExtensibleElements扩展。

correlationSet在tExtensibleElements的基础上，增加了2个属性

- properties
- name

# simpleType：QNames

```
<xsd:simpleType name="QNames">
    <xsd:restriction>
        <xsd:simpleType>
            <xsd:list itemType="xsd:QName"/>
        </xsd:simpleType>
        <xsd:minLength value="1"/>
    </xsd:restriction>
</xsd:simpleType>
```

QNames就是一个QName的list，最少1个，默认空格分割。

# element：faultHandlers

```
<xsd:element name="faultHandlers" type="tFaultHandlers"/>

<xsd:complexType name="tFaultHandlers">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="catch" minOccurs="0" maxOccurs="unbounded"/>
                <xsd:element ref="catchAll" minOccurs="0"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

faultHandlers，听名字就知道是干啥的了，同样支持tExtensibleElements扩展，由2种元素的sequence组成：

- catch
- catchAll

# element：catch

```
<xsd:element name="catch" type="tCatch">
    <xsd:annotation>
        <xsd:documentation>
This element can contain all activities including the activities compensate, compensateScope and rethrow.
        </xsd:documentation>
    </xsd:annotation>
</xsd:element>

<xsd:complexType name="tCatch">
    <xsd:complexContent>
        <xsd:extension base="tActivityContainer">
            <xsd:attribute name="faultName" type="xsd:QName"/>
            <xsd:attribute name="faultVariable" type="BPELVariableName"/>
            <xsd:attribute name="faultMessageType" type="xsd:QName"/>
            <xsd:attribute name="faultElement" type="xsd:QName"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

catch扩展自tActivityContainer，包含4个属性，对catch做了限定：

- faultName
- faultVariable
- faultMessageType
- faultElement

# element：catchAll

```
<xsd:element name="catchAll" type="tActivityContainer">
    <xsd:annotation>
        <xsd:documentation>
This element can contain all activities including the activities compensate, compensateScope and rethrow.
        </xsd:documentation>
    </xsd:annotation>
</xsd:element>

<xsd:complexType name="tActivityContainer">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

不同于catch，catchAll没有那么多属性（我全都要），就是tActivityContainer本尊，扩展自tExtensibleElements，同时包含一个activity的sequence。

# element：eventHandlers

```
<xsd:element name="eventHandlers" type="tEventHandlers"/>

<xsd:complexType name="tEventHandlers">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element onAlarm needs to be a Local Element Declaration, because there is another onAlarm element defined for the pick activity.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="onEvent" minOccurs="0" maxOccurs="unbounded"/>
                <xsd:element name="onAlarm" type="tOnAlarmEvent" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

eventHandlers基于tExtensibleElements扩展，由2种元素的sequence组成：

- onEvent：见element：onEvent
- onAlarm：见complexType：tOnAlarmEvent

# element：onEvent

```
<xsd:element name="onEvent" type="tOnEvent"/>

<xsd:complexType name="tOnEvent">
    <xsd:complexContent>
        <xsd:extension base="tOnMsgCommon">
            <xsd:sequence>
                <xsd:element ref="scope" minOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute name="messageType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="element" type="xsd:QName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

onEvent元素基于tOnMsgCommon进行扩展，包含一个element sequence：

- scope：用于表明作用范围

2个attribute：

- messageType
- element

# complexType：tOnMsgCommon

```
<xsd:complexType name="tOnMsgCommon">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlations needs to be a Local Element Declaration, because there is another correlations element defined for the invoke activity.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element name="correlations" type="tCorrelations" minOccurs="0"/>
                <xsd:element ref="fromParts" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="partnerLink" type="xsd:NCName" use="required"/>
            <xsd:attribute name="portType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="operation" type="xsd:NCName" use="required"/>
            <xsd:attribute name="messageExchange" type="xsd:NCName" use="optional"/>
            <xsd:attribute name="variable" type="BPELVariableName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

tOnMsgCommon这个complexType同样支持tExtensibleElements扩展，包含由两类element组成的sequence：

- correlations：type为tCorrelations
- fromParts

同时还引入了5种属性：

- partnerLink：必填的链接名
- portType：选填的端口类型
- operation：必填的操作
- messageExchange：选填的操作信息
- variable：选填的变量

# complexType：tCorrelations

```
<xsd:complexType name="tCorrelations">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlation needs to be a Local Element Declaration, because there is another correlation element defined for the invoke activity.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element name="correlation" type="tCorrelation" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:complexType name="tCorrelation">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="set" type="xsd:NCName" use="required"/>
            <xsd:attribute name="initiate" type="tInitiate" default="no"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:simpleType name="tInitiate">
    <xsd:restriction base="xsd:string">
        <xsd:enumeration value="yes"/>
        <xsd:enumeration value="join"/>
        <xsd:enumeration value="no"/>
    </xsd:restriction>
</xsd:simpleType>
```

tCorrelations基于tExtensibleElements扩展，由1至多个correlation的sequence组成。

tCorrelation同样基于tExtensibleElements扩展，在此之上还定义了两个属性：

- set
- initiate：yes | join | no

# complexType：tOnAlarmEvent

```
<xsd:complexType name="tOnAlarmEvent">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:choice>
                    <xsd:sequence>
                        <xsd:group ref="forOrUntilGroup" minOccurs="1"/>
                        <xsd:element ref="repeatEvery" minOccurs="0"/>
                    </xsd:sequence>
                    <xsd:element ref="repeatEvery" minOccurs="1"/>
                </xsd:choice>
                <xsd:element ref="scope" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

tOnAlarmEvent基于tExtensibleElements扩展，由1个group：forOrUntilGroup 和 0或1个element：repeatEvery组成的sequence，或者1个element：repeatEvery，再加上一个scope组成的sequence组成。

三个元素都通过ref引用，可以继续往后看定义。

# group：forOrUntilGroup

```
<xsd:group name="forOrUntilGroup">
    <xsd:choice>
        <xsd:element ref="for" minOccurs="1"/>
        <xsd:element ref="until" minOccurs="1"/>
    </xsd:choice>
</xsd:group>

<xsd:element name="for" type="tDuration-expr"/>

<xsd:element name="until" type="tDeadline-expr"/>

<xsd:element name="repeatEvery" type="tDuration-expr"/>
```

可以看到，forOrUntilGroup还真就是for或者until两个元素中选一个。这两个东西又分别通过tDuration-expr，tDeadline-expr来定义。

刚好element：repeatEvery也在这后边，一起讲了吧，同样通过tDuration-expr来定义。

# complexType：tActivity

```
<xsd:complexType name="tActivity">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="targets" minOccurs="0"/>
                <xsd:element ref="sources" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="name" type="xsd:NCName"/>
            <xsd:attribute name="suppressJoinFailure" type="tBoolean" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

一看是activity的type，感觉应该出现过，其实并没有，大概是后边会用到吧。

同样基于tExtensibleElements扩展（啥都要tExtensibleElements扩展一下，这兼容性也太强了吧），包含两个元素组成的sequence

- targets：出现0或1次
- sources：出现0或1次

另外还有俩属性：

- name
- suppressJoinFailure：有一种故障叫joinFailure，在连接条件求值为 false 时抛出。通过将流程或活动属性 suppressJoinFailure 设置为 yes，可以禁止此故障。

# element：targets

```
<xsd:element name="targets" type="tTargets"/>

<xsd:complexType name="tTargets">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="joinCondition" minOccurs="0"/>
                <xsd:element ref="target" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

targets基于tExtensibleElements进行扩展，包含两个元素组成的sequence：

- joinCondition：0或1个
- target：1到多个

# element：joinCondition

```
<xsd:element name="joinCondition" type="tCondition"/>
```

这玩意儿的定义在tCondition里边，其实装的就是几乎啥都可以写的混合内容。

# element：target

```
<xsd:element name="target" type="tTarget"/>

<xsd:complexType name="tTarget">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="linkName" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

tTarget这个东西是一个机遇tExtensibleElements扩展的元素，就增加了一个元素

- linkName：NCName，必填，写上你的目标

# element：sources

```
<xsd:element name="sources" type="tSources"/>

<xsd:complexType name="tSources">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="source" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="source" type="tSource"/>

<xsd:complexType name="tSource">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="transitionCondition" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="linkName" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="transitionCondition" type="tCondition"/>
```

sources：tExtensibleElements，以及1到多个source

source：tExtensibleElements扩展，还有一个元素的sequence

- transitionCondition：转移条件，可以不出现，也可以出现一次

还有一个属性

- linkName：NCName，必填，写上你的来源

# element：assign

```
<xsd:element name="assign" type="tAssign"/>

<xsd:complexType name="tAssign">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:choice maxOccurs="unbounded">
                    <xsd:element ref="copy" minOccurs="1"/>
                    <xsd:element ref="extensionAssignOperation" minOccurs="1"/>
                </xsd:choice>
            </xsd:sequence>
            <xsd:attribute name="validate" type="tBoolean" use="optional" default="no"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

Assign，基于tActivity扩展，增加了两个属性，二选一只猴构成sequence

- copy：出现1至多次
- extensionAssignOperation：出现1至多次

还增加了一个属性

- validate：bool值，默认是no，可选

# element：copy

```
<xsd:element name="copy" type="tCopy"/>

<xsd:complexType name="tCopy">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="from" minOccurs="1"/>
                <xsd:element ref="to" minOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute name="keepSrcElementName" type="tBoolean" use="optional" default="no"/>
            <xsd:attribute name="ignoreMissingFromData" type="tBoolean" use="optional" default="no"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

copy，基于tExtensibleElements扩展，两个元素比较好理解

- from：从哪里copy
- to：copy到哪里

还有两个属性：

- keepSrcElementName：bool值，可选，默认no，是否保存源元素的属性名
- ignoreMissingFromData：bool值，可选，默认no，是否忽略数据中的遗失部分

# element：from

```
<xsd:element name="from" type="tFrom"/>

<xsd:complexType name="tFrom" mixed="true">
    <xsd:sequence>
        <xsd:element ref="documentation" minOccurs="0" maxOccurs="unbounded"/>
        <xsd:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
        <xsd:choice minOccurs="0">
            <xsd:element ref="literal" minOccurs="1"/>
            <xsd:element ref="query" minOccurs="1"/>
        </xsd:choice>
    </xsd:sequence>
    <xsd:attribute name="expressionLanguage" type="xsd:anyURI"/>
    <xsd:attribute name="variable" type="BPELVariableName"/>
    <xsd:attribute name="part" type="xsd:NCName"/>
    <xsd:attribute name="property" type="xsd:QName"/>
    <xsd:attribute name="partnerLink" type="xsd:NCName"/>
    <xsd:attribute name="endpointReference" type="tRoles"/>
    <xsd:anyAttribute namespace="##other" processContents="lax"/>
</xsd:complexType>
```

然而这个from并不简单，我们先看看这个sequence里边装了什么

- documentation：element，0至多个，前边定义过了，基本上就是一个mixed的啥都可以写的东西，属性里边指明source和language就可以了
- any：0至多个来自其他命名空间的任意元素
- literal/query：这俩二选一，具体是啥看后边定义

还有几个属性

- expressionLanguage：表达语言
- variable：BPELVariableName
- part：来自哪一部分
- property：属性是什么
- partnerLink
- endpointReference：通过tRoles定义
- anyAttribute：还可以随便加其他属性

# element：literal

```
<xsd:element name="literal" type="tLiteral"/>

<xsd:complexType name="tLiteral" mixed="true">
    <xsd:sequence>
        <xsd:any namespace="##any" processContents="lax" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
</xsd:complexType>
```

literal就是一个mixed描述段落，里边可以有一个任意元素。

# element：query

```
<xsd:element name="query" type="tQuery"/>

<xsd:complexType name="tQuery" mixed="true">
    <xsd:sequence>
        <xsd:any processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xsd:sequence>
    <xsd:attribute name="queryLanguage" type="xsd:anyURI"/>
    <xsd:anyAttribute namespace="##other" processContents="lax"/>
</xsd:complexType>
```

Query和literal相似，是一个mixed描述段落，里边可以有任意个任意元素。同时它还有属性

- queryLanguage：表明查询语言
- anyAttribute：看似随便加属性，实际上根据查询语言不通增加其他属性

# simpleType：tRoles

```
<xsd:simpleType name="tRoles">
    <xsd:restriction base="xsd:string">
        <xsd:enumeration value="myRole"/>
        <xsd:enumeration value="partnerRole"/>
    </xsd:restriction>
</xsd:simpleType>
```

myRole和partnerRole两个值二选一

# element：to

```
<xsd:element name="to" type="tTo"/>

<xsd:complexType name="tTo" mixed="true">
    <xsd:sequence>
        <xsd:element ref="documentation" minOccurs="0" maxOccurs="unbounded"/>
        <xsd:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
        <xsd:element ref="query" minOccurs="0"/>
    </xsd:sequence>
    <xsd:attribute name="expressionLanguage" type="xsd:anyURI"/>
    <xsd:attribute name="variable" type="BPELVariableName"/>
    <xsd:attribute name="part" type="xsd:NCName"/>
    <xsd:attribute name="property" type="xsd:QName"/>
    <xsd:attribute name="partnerLink" type="xsd:NCName"/>
    <xsd:anyAttribute namespace="##other" processContents="lax"/>
</xsd:complexType>
```

这个to也不简单，我们看看这个sequence里边装了什么

- documentation：element，0至多个，前边定义过了，基本上就是一个mixed的啥都可以写的东西，属性里边指明source和language就可以了
- any：0至多个来自其他命名空间的任意元素
- query：查询到对应写入的部分

还有几个属性

- expressionLanguage：表达语言
- variable：BPELVariableName
- part：去哪一部分
- property：属性是什么
- partnerLink
- anyAttribute：还可以根据表达语言随便加其他属性

# element：extensionAssignOperation

```
<xsd:element name="extensionAssignOperation" type="tExtensionAssignOperation"/>

<xsd:complexType name="tExtensionAssignOperation">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements"/>
    </xsd:complexContent>
</xsd:complexType>
```

这个东西是assign中的一个element，怪不得assign没有基于tExtensibleElements扩展，放在里边了。

# element：compensate

```
<xsd:element name="compensate" type="tCompensate"/>

<xsd:complexType name="tCompensate">
    <xsd:complexContent>
        <xsd:extension base="tActivity"/>
    </xsd:complexContent>
</xsd:complexType>
```

就是tActivity的一个元素化，可以看作是一个最原始，最纯粹的activity。

# element：compensateScope

```
<xsd:element name="compensateScope" type="tCompensateScope"/>

<xsd:complexType name="tCompensateScope">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:attribute name="target" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

在compensate之上增加了一个属性：

- target：相当于给补偿增加了一个范围

# element：empty

```
<xsd:element name="empty" type="tEmpty"/>

<xsd:complexType name="tEmpty">
    <xsd:complexContent>
        <xsd:extension base="tActivity"/>
    </xsd:complexContent>
</xsd:complexType>
```

就是tActivity的一个元素化，可以看作是一个最原始，最纯粹的activity。

和compensate是一样的，在语义上和用法上不一样。

# element：exit

```
<xsd:element name="exit" type="tExit"/>

<xsd:complexType name="tExit">
    <xsd:complexContent>
        <xsd:extension base="tActivity"/>
    </xsd:complexContent>
</xsd:complexType>
```

就是tActivity的一个元素化，可以看作是一个最原始，最纯粹的activity。

和compensate、empty定义是一样的，在语义上和用法上不一样。

# element：extensionActivity

```
<xsd:element name="extensionActivity" type="tExtensionActivity"/>

<xsd:complexType name="tExtensionActivity">
    <xsd:sequence>
        <xsd:any namespace="##other" processContents="lax"/>
    </xsd:sequence>
</xsd:complexType>
```

允许从其他命名空间增加元素当作activity装进来

# element：flow

```
<xsd:element name="flow" type="tFlow"/>

<xsd:complexType name="tFlow">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="links" minOccurs="0"/>
                <xsd:group ref="activity" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

基于tActivity进行扩展，sequence中包含：

- element links：可以没有，在后边定义
- group activity：1到多个activity的group，算是一个嵌套的定义

# element：links

```
<xsd:element name="links" type="tLinks"/>

<xsd:complexType name="tLinks">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="link" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="link" type="tLink"/>

<xsd:complexType name="tLink">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="name" type="xsd:NCName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

links基于tExtensibleElements进行扩展（辣个男人又回来了），包含1至多个link

link同样基于tExtensibleElements进行扩展，增加了一个属性

name：NCName，这就是link的真相了

# element：forEach

```
<xsd:element name="forEach" type="tForEach"/>

<xsd:complexType name="tForEach">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="startCounterValue" minOccurs="1"/>
                <xsd:element ref="finalCounterValue" minOccurs="1"/>
                <xsd:element ref="completionCondition" minOccurs="0"/>
                <xsd:element ref="scope" minOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute name="counterName" type="BPELVariableName" use="required"/>
            <xsd:attribute name="parallel" type="tBoolean" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="startCounterValue" type="tExpression"/>

<xsd:element name="finalCounterValue" type="tExpression"/>
```

看名字就知道是一个forEach循环了，element包括

- startCounterValue
- finalCounterValue
- completionCondition
- scope

这四个应该不用解释了吧，for循环要素

然后还有两个属性

- counterName：计数器的名称
- parallel：是否并行

# element：completionCondition

```
<xsd:element name="completionCondition" type="tCompletionCondition"/>

<xsd:complexType name="tCompletionCondition">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="branches" minOccurs="0"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

这个condition由一组branches构成

# element：branches

```
<xsd:element name="branches" type="tBranches"/>

<xsd:complexType name="tBranches">
    <xsd:complexContent>
        <xsd:extension base="tExpression">
            <xsd:attribute name="successfulBranchesOnly" type="tBoolean" default="no"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

branches基于tExpression进行扩展，还有一个属性

- successfulBranchesOnly：默认是no

# element：if

```
<xsd:element name="if" type="tIf"/>

<xsd:complexType name="tIf">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="condition" minOccurs="1"/>
                <xsd:group ref="activity" minOccurs="1"/>
                <xsd:element ref="elseif" minOccurs="0" maxOccurs="unbounded"/>
                <xsd:element ref="else" minOccurs="0"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

if也是一类activity，基于tAcitivity进行扩展，sequence中包括

- condition：element，至少一个
- activity：group，至少一个
- elseif：element，0到多个
- else：element，可以没有

# element：elseif

```
<xsd:element name="elseif" type="tElseif"/>

<xsd:complexType name="tElseif">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="condition" minOccurs="1"/>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="else" type="tActivityContainer"/>
```

elseif基于tExtensibleElements扩展，sequence中包括

- condition：element，数量1
- activity：group，数量1

Else就直接是tActivityContainer了，condition都不用。

# element：invoke

```
<xsd:element name="invoke" type="tInvoke"/>

<xsd:complexType name="tInvoke">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlations needs to be a Local Element Declaration, because there is another correlations element defined for the non-invoke activities.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element name="correlations" type="tCorrelationsWithPattern" minOccurs="0"/>
                <xsd:element ref="catch" minOccurs="0" maxOccurs="unbounded"/>
                <xsd:element ref="catchAll" minOccurs="0"/>
                <xsd:element ref="compensationHandler" minOccurs="0"/>
                <xsd:element ref="toParts" minOccurs="0"/>
                <xsd:element ref="fromParts" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="partnerLink" type="xsd:NCName" use="required"/>
            <xsd:attribute name="portType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="operation" type="xsd:NCName" use="required"/>
            <xsd:attribute name="inputVariable" type="BPELVariableName" use="optional"/>
            <xsd:attribute name="outputVariable" type="BPELVariableName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

annotation说明就不讲了

基于tActivity扩展，用调用其他 Web 服务，element有6个

- correlations：本地的元素声明
- catch
- catchAll
- compensationHandler
- toParts
- fromParts

attribute有5个

- partnerLink
- portType
- operation
- inputVariable
- outputVariable

感觉看名字就懂了，没啥好讲的

# complexType：tCorrelationsWithPattern

```
<xsd:complexType name="tCorrelationsWithPattern">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlation needs to be a Local Element Declaration, because there is another correlation element defined for the non-invoke activities.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element name="correlation" type="tCorrelationWithPattern" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:complexType name="tCorrelationWithPattern">
    <xsd:complexContent>
        <xsd:extension base="tCorrelation">
            <xsd:attribute name="pattern" type="tPattern"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:simpleType name="tPattern">
    <xsd:restriction base="xsd:string">
        <xsd:enumeration value="request"/>
        <xsd:enumeration value="response"/>
        <xsd:enumeration value="request-response"/>
    </xsd:restriction>
</xsd:simpleType>
```

一通定义，实际上这个tCorrelationsWithPattern就是1至多个tCorrelationWithPattern

这个tCorrelationWithPattern从tCorrelation进行扩展，增加了属性pattern

这个pattern就是request、response、request-response三选一

# element：fromParts

```
<xsd:element name="fromParts" type="tFromParts"/>

<xsd:complexType name="tFromParts">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="fromPart" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="fromPart" type="tFromPart"/>

<xsd:complexType name="tFromPart">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="part" type="xsd:NCName" use="required"/>
            <xsd:attribute name="toVariable" type="BPELVariableName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

fromParts扩展自tExtensibleElements，由1到多个fromPart组成

fromPart同样扩展自tExtensibleElements，包含两个attribute

- part
- toVariable

# element：toParts

```
<xsd:element name="toParts" type="tToParts"/>

<xsd:complexType name="tToParts">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:element ref="toPart" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>

<xsd:element name="toPart" type="tToPart"/>

<xsd:complexType name="tToPart">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:attribute name="part" type="xsd:NCName" use="required"/>
            <xsd:attribute name="fromVariable" type="BPELVariableName" use="required"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

toParts扩展自tExtensibleElements，由1到多个toPart组成

toPart同样扩展自tExtensibleElements，包含两个attribute

- part
- fromVariable

# element：pick

```
<xsd:element name="pick" type="tPick"/>

<xsd:complexType name="tPick">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element onAlarm needs to be a Local Element Declaration, because there is another onAlarm element defined for event handlers.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="onMessage" minOccurs="1" maxOccurs="unbounded"/>
                <xsd:element name="onAlarm" type="tOnAlarmPick" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute name="createInstance" type="tBoolean" default="no"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

pick扩展自tActivity

两个属性都在后边定义：

- onMessage
- onAlarm

一个属性：

- createInstance

# element：onMessage

```
<xsd:element name="onMessage" type="tOnMessage"/>

<xsd:complexType name="tOnMessage">
    <xsd:complexContent>
        <xsd:extension base="tOnMsgCommon">
            <xsd:sequence>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tOnMsgCommon

sequence中是一个activity的group

# complexType：tOnAlarmPick

```
<xsd:complexType name="tOnAlarmPick">
    <xsd:complexContent>
        <xsd:extension base="tExtensibleElements">
            <xsd:sequence>
                <xsd:group ref="forOrUntilGroup" minOccurs="1"/>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tExtensibleElements

sequence中有两个group

- forOrUntilGroup
- activity

都是出现一次

# element：receive

```
<xsd:element name="receive" type="tReceive"/>

<xsd:complexType name="tReceive">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlations needs to be a Local Element Declaration, because there is another correlations element defined for the invoke activity.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element name="correlations" type="tCorrelations" minOccurs="0"/>
                <xsd:element ref="fromParts" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="partnerLink" type="xsd:NCName" use="required"/>
            <xsd:attribute name="portType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="operation" type="xsd:NCName" use="required"/>
            <xsd:attribute name="variable" type="BPELVariableName" use="optional"/>
            <xsd:attribute name="createInstance" type="tBoolean" default="no"/>
            <xsd:attribute name="messageExchange" type="xsd:NCName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，用于（接收请求）等待客户端通过发送消息调用业务流程

2个element

- correlations
- fromParts

6个attribute

- partnerLink
- portType
- operation
- variable
- createInstance
- messageExchange

# element：repeatUntil

```
<xsd:element name="repeatUntil" type="tRepeatUntil"/>

<xsd:complexType name="tRepeatUntil">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:group ref="activity" minOccurs="1"/>
                <xsd:element ref="condition" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity

sequence中包含

- activity：group，数量为1
- condition：element，数量也是1

# element：reply

```
<xsd:element name="reply" type="tReply"/>

<xsd:complexType name="tReply">
    <xsd:annotation>
        <xsd:documentation>
XSD Authors: The child element correlations needs to be a Local Element Declaration, because there is another correlations element defined for the invoke activity.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element name="correlations" type="tCorrelations" minOccurs="0"/>
                <xsd:element ref="toParts" minOccurs="0"/>
            </xsd:sequence>
            <xsd:attribute name="partnerLink" type="xsd:NCName" use="required"/>
            <xsd:attribute name="portType" type="xsd:QName" use="optional"/>
            <xsd:attribute name="operation" type="xsd:NCName" use="required"/>
            <xsd:attribute name="variable" type="BPELVariableName" use="optional"/>
            <xsd:attribute name="faultName" type="xsd:QName"/>
            <xsd:attribute name="messageExchange" type="xsd:NCName" use="optional"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，用于生成同步操作的响应

2个element

- correlations
- toParts

6个attribute

- partnerLink
- portType
- operation
- variable
- faultName
- messageExchange

# element：rethrow

```
<xsd:element name="rethrow" type="tRethrow"/>

<xsd:complexType name="tRethrow">
    <xsd:complexContent>
        <xsd:extension base="tActivity"/>
    </xsd:complexContent>
</xsd:complexType>
```

就是tActivity的一个元素化，可以看作是一个最原始，最纯粹的activity。

和compensate、empty，exit定义是一样的，就是在语义上和用法上不一样。

# element：scope

```
<xsd:element name="scope" type="tScope"/>

<xsd:complexType name="tScope">
    <xsd:annotation>
        <xsd:documentation>
There is no schema-level default for "exitOnStandardFault" at "scope". Because, it will inherit default from enclosing scope or process.
        </xsd:documentation>
    </xsd:annotation>
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="partnerLinks" minOccurs="0"/>
                <xsd:element ref="messageExchanges" minOccurs="0"/>
                <xsd:element ref="variables" minOccurs="0"/>
                <xsd:element ref="correlationSets" minOccurs="0"/>
                <xsd:element ref="faultHandlers" minOccurs="0"/>
                <xsd:element ref="compensationHandler" minOccurs="0"/>
                <xsd:element ref="terminationHandler" minOccurs="0"/>
                <xsd:element ref="eventHandlers" minOccurs="0"/>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute name="isolated" type="tBoolean" default="no"/>
            <xsd:attribute name="exitOnStandardFault" type="tBoolean"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，用于以分层方式将复杂流程划分为多个组织部分。scope为活动提供了行为上下文。换言之，scope可以为不同的活动（或在 <sequence> 或 <flow>) 等通用的结构化活动下收集的活动集）定义不同的故障处理程序。除了定义故障处理程序以外，scope还可以声明只在作用域中可见的变量。scope还可以定义本地关联集、补偿处理程序和事件处理程序。

sequence中有：

- partnerLinks
- messageExchanges
- variables
- correlationSets
- faultHandlers
- compensationHandler
- terminationHandler
- eventHandlers
- activity

还有两个属性  

- isolated
- exitOnStandardFault

# element：compensationHandler

```
<xsd:element name="compensationHandler" type="tActivityContainer">
    <xsd:annotation>
        <xsd:documentation>
This element can contain all activities including the activities compensate and compensateScope.
        </xsd:documentation>
    </xsd:annotation>
</xsd:element>
```

实际上就是一个tActivityContainer

# element：terminationHandler

```
<xsd:element name="terminationHandler" type="tActivityContainer">
    <xsd:annotation>
        <xsd:documentation>
This element can contain all activities including the activities compensate and compensateScope.
        </xsd:documentation>
    </xsd:annotation>
</xsd:element>
```

实际上就是一个tActivityContainer

# element：sequence

```
<xsd:element name="sequence" type="tSequence"/>

<xsd:complexType name="tSequence">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:group ref="activity" minOccurs="1" maxOccurs="unbounded"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，但是里边还可以装很多的activity的group的序列

# element：throw

```
<xsd:element name="throw" type="tThrow"/>

<xsd:complexType name="tThrow">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:attribute name="faultName" type="xsd:QName" use="required"/>
            <xsd:attribute name="faultVariable" type="BPELVariableName"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，增加了两个属性

- faultName
- faultVariable

# element：validate

```
<xsd:element name="validate" type="tValidate"/>

<xsd:complexType name="tValidate">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:attribute name="variables" use="required" type="BPELVariableNames"/>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，增加了1个属性

- variables：BPELVariableNames

# simpleType：BPELVariableNames

```
<xsd:simpleType name="BPELVariableNames">
    <xsd:restriction>
        <xsd:simpleType>
            <xsd:list itemType="BPELVariableName"/>
        </xsd:simpleType>
        <xsd:minLength value="1"/>
    </xsd:restriction>
</xsd:simpleType>
```

一个BPELVariableName的list，默认空格分割，最短一个

# element：wait

```
<xsd:element name="wait" type="tWait"/>

<xsd:complexType name="tWait">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:choice>
                <xsd:element ref="for" minOccurs="1"/>
                <xsd:element ref="until" minOccurs="1"/>
            </xsd:choice>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，增加了元素，从for和until中二选一

# element：while

```
<xsd:element name="while" type="tWhile"/>

<xsd:complexType name="tWhile">
    <xsd:complexContent>
        <xsd:extension base="tActivity">
            <xsd:sequence>
                <xsd:element ref="condition" minOccurs="1"/>
                <xsd:group ref="activity" minOccurs="1"/>
            </xsd:sequence>
        </xsd:extension>
    </xsd:complexContent>
</xsd:complexType>
```

扩展自tActivity，增加了一个sequence，里边有

- condition
- activity

# complexType：tExpression

```
<xsd:complexType name="tExpression" mixed="true">
    <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
    </xsd:sequence>
    <xsd:attribute name="expressionLanguage" type="xsd:anyURI"/>
    <xsd:anyAttribute namespace="##other" processContents="lax"/>
</xsd:complexType>
```

tExpression的内容基本就是随便写

属性有一个expressionLanguage，然后还可以再增加其他属性

看起来是any，实际上要看expressionLanguage的

# complexType：tCondition

```
<xsd:complexType name="tCondition">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

tCondition就是在tExpression的基础上随便写

# element：condition

```
<xsd:element name="condition" type="tBoolean-expr"/>

<xsd:complexType name="tBoolean-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

condition也是在tExpression的基础上随便写

# complexType：tDuration-expr

```
<xsd:complexType name="tDuration-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

在tExpression的基础上随便写

# complexType：tDeadline-expr

```
<xsd:complexType name="tDeadline-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

在tExpression的基础上随便写

# simpleType：tBoolean

```
<xsd:simpleType name="tBoolean">
    <xsd:restriction base="xsd:string">
        <xsd:enumeration value="yes"/>
        <xsd:enumeration value="no"/>
    </xsd:restriction>
</xsd:simpleType>
```

yes or no，二选一

