---
title: WS-BPEL 2.0 xml schema 详解(一)
date: 2019-03-21 22:11:55
tags: [BPEL, WSBPEL, ws-bpel, BPEL2.0, Schema, XSD]
---

本系列文章将一行一行的解读wsbpel2.0的源码。

相关xsd语法问题，请参见[XSD学习笔记完整版](https://imonce.github.io/2019/03/15/XSD学习笔记完整版/#简单类型)

wsbpel2.0 xsd源码来自：[ws-bpel_executable.xsd](http://docs.oasis-open.org/wsbpel/2.0/OS/process/executable/ws-bpel_executable.xsd)

# schema声明

```xml
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

```xml
<xsd:annotation>
	<xsd:documentation>
Schema for Executable Process for WS-BPEL 2.0 OASIS Standard 11th April, 2007
	</xsd:documentation>
</xsd:annotation>
```

赠送了一个简单的文档说明。

# import

```xml
<xsd:import namespace="http://www.w3.org/XML/1998/namespace" schemaLocation="http://www.w3.org/2001/xml.xsd"/>
```

引入`http://www.w3.org/2001/xml.xsd`的xml语素，前缀默认为xml。

# element：process

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

|基本活动名称|释义|
|:--|:--|
|assign|活动的作用是用新的数据来更新变量的值。Assign活动可以包括任意数量的基本复制操作。|
|compensate|通过该活动做一些补偿动作，通常需要和scope联合使用。只能从故障处理程序或另一个补偿处理活动中调用这个活动。补偿处理程序只能被调用一次。|
|compensateScope||
|empty|无所事事，比如在一个错误发生后可以不做反应来消除这个错误|
|exit|该活动用于立刻终止业务流程实例。所有当前运行的活动必须被立刻终止。不用引用任何终点处理、错误处理或者补偿行为。|
|forEach||
|invoke|活动允许业务流程同步或异步调用由合作伙伴提供的服务，服务实现可以是单向或请求-响应操作。Invoke活动使用“partnerLink”来引用伙伴服务。同过“portType”和“operation”指定相应的WSDL接口和操作。|
|pick|活动会等待一组相互排斥事件中的一个事件的发生，然后执行与发生的事件相关联的活动。它会阻塞业务流程执行，以等待某一特定的事件发生，比如接收到一个合适的消息或超时警报响起。当其中任何一个事件被触发后，业务流程就会继续执行，pick也随即完成了，不会再等待其他事件的发生。|
|receive|活动从流程的外部伙伴那获取数据，并将其保存到流程变量。通常一个Receive是一个流程的初始点，它会阻塞执行直到匹配的消息的到达。|
|reply|活动发送消息给伙伴来应答通过receive活动所接收到的消息。receive和reply的组合对应着WSDL portType上定义的一个请求-响应操作。如果receive活动对应着一个单向(one-way)操作，则不能在流程中定义对应的reply活动。|
|rethrow||
|throw|提示一个错误，一个故障处理可以处理这样的错误。假如一个错误不被处理的话它最终到达最高层后导致过程的终止|
|validate||
|wait|活动会暂停流程执行，等待一段给定的时间或等到某一时刻才继续运行。在WebSphere Process Server 6.0中，开发者可以非常灵活地指定wait中的到期条件，比如等待多少秒，等到特定的一个日期，或是使用内置的日期表现法。也可以使用Java代码来动态指定等待时间。|

|结构化活动名称|释义|
|:--|:--|
|extensionActivity||
|flow|可以描述更为复杂的活动执行顺序。我们可以利用flow指定一个或多个并行执行的活动。为了定义任意的控制结构，可以在并行的活动中使用链接。|
|if||
|repeatUntil||
|scope|使用这个结构可以将一组活动组织在一起作为一个处理单位。通过这个组织方法多个活动可以使用同一个故障处理、事故处理和补偿处理。通过补偿处理BPEL可以处理长时间的处理。|
|sequence|定义一组按顺序先后执行的活动。执行顺序是sequence活动中嵌套活动的先后顺序。当sequence中的最后一个活动完成后，该sequence活动也就完成了。|
|while|继承于传统的结构化编程思想，提供了while-do循环结构的支持。它可以包含一个或多个活动。它指定反复执行其内部活动，直到成功条件不被满足为止。在WPS中允许其使用Java代码来描述条件表达式。|

# element：extensions

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
<xsd:simpleType name="BPELVariableName">
    <xsd:restriction base="xsd:NCName">
        <xsd:pattern value="[^\.]+"/>
    </xsd:restriction>
</xsd:simpleType>
```

bpel变量的一个限制，BPELVariableName需要满足xsd:NCName限制，不能以`.`开头，且长度大于等于一个字符。

# element：correlationSets

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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


