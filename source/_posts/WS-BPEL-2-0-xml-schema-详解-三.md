---
title: WS-BPEL 2.0 xml schema 详解(三)
date: 2019-03-22 16:24:20
tags: [bpel, wsbpel, ws-bpel, bpel2.0, schema, xsd]
---

# element：exit

```xml
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

```xml
<xsd:element name="extensionActivity" type="tExtensionActivity"/>

<xsd:complexType name="tExtensionActivity">
    <xsd:sequence>
        <xsd:any namespace="##other" processContents="lax"/>
    </xsd:sequence>
</xsd:complexType>
```

允许从其他命名空间增加元素当作activity装进来

# element：flow

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
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

```xml
<xsd:complexType name="tCondition">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

tCondition就是在tExpression的基础上随便写

# element：condition

```xml
<xsd:element name="condition" type="tBoolean-expr"/>

<xsd:complexType name="tBoolean-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

condition也是在tExpression的基础上随便写

# complexType：tDuration-expr

```xml
<xsd:complexType name="tDuration-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

在tExpression的基础上随便写

# complexType：tDeadline-expr

```xml
<xsd:complexType name="tDeadline-expr">
    <xsd:complexContent mixed="true">
        <xsd:extension base="tExpression"/>
    </xsd:complexContent>
</xsd:complexType>
```

在tExpression的基础上随便写

# simpleType：tBoolean

```xml
<xsd:simpleType name="tBoolean">
    <xsd:restriction base="xsd:string">
        <xsd:enumeration value="yes"/>
        <xsd:enumeration value="no"/>
    </xsd:restriction>
</xsd:simpleType>
```

yes or no，二选一

