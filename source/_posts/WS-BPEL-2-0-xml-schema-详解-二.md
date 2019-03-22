---
title: WS-BPEL 2.0 xml schema 详解(二)
date: 2019-03-22 16:24:10
tags: [bpel, wsbpel, ws-bpel, bpel2.0, schema, xsd]
---

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