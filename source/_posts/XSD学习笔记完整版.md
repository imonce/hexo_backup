---
title: XSD/XML Schema 学习笔记完整版
date: 2019-03-15 15:12:45
tags: [XSD, XML Schema, XML, Schema, 学习笔记]
---

> **注：**本文摘自[W3school Schema 教程](http://www.w3school.com.cn/schema/index.asp)

# schema声明

属性大全

|属性|说明|取值|
|:--|:--|:--|
|id|标识该元素的唯一ID| |
|attributeFormDefault|指定XML文档使用schema中定义的局部属性时是否必须使用命名空间限定|qualified:必须通过命名空间前缀限定、unqualified：（默认值）无须通过命名空间前缀限定|
|elementFormDefault|指定XML文档使用schema中定义的局部元素时是否必须使用命名空间限定|取值和含义同attributeFormDefault|
|blockDefault|设定schema中element和complexType上的block属性的默认值、block属性用来阻止以指定的派生类型代替原类型|\#all或者extension、restriction和substitution的自由组合、例如extension表示防止通过扩展派生的复杂类型替代该复杂类型|
|finalDefault|设定schema中element、simpleType和complexType上的final的默认值、final属性用来阻止以指定的派生类型来派生新类型|对于element和complexType：值可以是#all或extension和restriction的自由组合、对于simpleType：值可以是#all或restriction、list和union的自由组合|
|targetNamespace|设定schema的命名空间的URI引用| |
|version|设定schema的版本| |
|xmlns|设定schema使用的一个或多个命名空间的URI引用| |
|any attributes|设定带有non-schema命名空间的任何其他属性| |

## <schema\> 声明

<schema\> 元素是每一个 XML Schema 的根元素：
<?xml version="1.0"?\>

```
<xs:schema>

...
...

</xs:schema>
```

<schema\> 元素可包含属性。一个 schema 声明往往看上去类似这样：

```
<?xml version="1.0"?>
 
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema"
targetNamespace="http://www.w3school.com.cn"
xmlns="http://www.w3school.com.cn"
elementFormDefault="qualified">

...
...
</xs:schema>
```

### 代码解释

```
xmlns:xs="http://www.w3.org/2001/XMLSchema"
```

显示 schema 中用到的元素和数据类型来自命名空间 `http://www.w3.org/2001/XMLSchema`。

同时它还规定了来自命名空间 `http://www.w3.org/2001/XMLSchema`的元素和数据类型应该使用前缀 xs。

```
targetNamespace="http://www.w3school.com.cn" 
```

显示被此 schema 定义的元素 (note, to, from, heading, body) 来自命名空间： `http://www.w3school.com.cn`。

```
xmlns="http://www.w3school.com.cn" 
```

指出默认的命名空间是 `http://www.w3school.com.cn`。

```
elementFormDefault="qualified" 
```

指出任何 XML 实例文档所使用的且在此 schema 中声明过的元素必须被命名空间限定。

## 在 XSD 文档中引用 其他Schema

引用方式有两种：

- include
- import

import与include的作用是一样的。 区别在于import是导入另外一个命名空间的xsd， 而inlude是包含同一个命名空间的xsd。

### 例子

```
<xsd:include schemaLocation="module/owl1-lite-core.xsd" /> 
```

```
<xsd:import namespace="http://www.w3.org/XML/1998/namespace" 
            schemaLocation="xml.xsd">
  <!-- "http://www.w3.org/2001/xml.xsd" -->
  <xsd:annotation>
    <xsd:documentation>
      Get access to the xml: attribute groups for xml:lang
      as declared on 'Label' and 'Documentation' below 
    </xsd:documentation>
  </xsd:annotation>
</xsd:import>
```

## 在 XML 文档中引用 Schema

此 XML 文档含有对 XML Schema 的引用：

```
<?xml version="1.0"?>

<note xmlns="http://www.w3school.com.cn"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.w3school.com.cn note.xsd">

<to>George</to>
<from>John</from>
<heading>Reminder</heading>
<body>Don't forget the meeting!</body>
</note>
```

### 代码解释

```
xmlns="http://www.w3school.com.cn" 
```

规定了默认命名空间的声明。此声明会告知 schema 验证器，在此 XML 文档中使用的所有元素都被声明于 `http://www.w3school.com.cn` 这个命名空间。

一旦您拥有了可用的 XML Schema 实例命名空间：

```
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
```

您就可以使用 schemaLocation 属性了。此属性有两个值。第一个值是需要使用的命名空间。第二个值是供命名空间使用的 XML schema 的位置：

```
xsi:schemaLocation="http://www.w3school.com.cn note.xsd"
```

# 简单类型

## 元素

```
<xs:element name="xxx" type="yyy"/>
```

### 常用类型：

- xs:string
- xs:decimal
- xs:integer
- xs:boolean
- xs:date
- xs:time

### 默认值和固定值

缺省值（默认值）设置：

```
<xs:element name="color" type="xs:string" default="red"/>
```

固定值设置：

```
<xs:element name="color" type="xs:string" fixed="red"/>
```

### 例子

这是一些 XML 元素：

```
<lastname>Smith</lastname>
<age>28</age>
<dateborn>1980-03-27</dateborn>
```

这是相应的简易元素定义：

```
<xs:element name="lastname" type="xs:string"/>
<xs:element name="age" type="xs:integer"/>
<xs:element name="dateborn" type="xs:date"/>
```

## 属性

```
<xs:attribute name="xxx" type="yyy"/>
```

### 常用类型：

- xs:string
- xs:decimal
- xs:integer
- xs:boolean
- xs:date
- xs:time

### 例子

这是带有属性的 XML 元素：

```
<lastname lang="EN">Smith</lastname>
```

这是对应的属性定义：

```
<xs:attribute name="lang" type="xs:string"/>
```

### 默认值和固定值

缺省值（默认值）设置：

```
<xs:attribute name="lang" type="xs:string" default="EN"/>
```

固定值设置：

```
<xs:attribute name="lang" type="xs:string" fixed="EN"/>
```

### 可选和必选

在缺省的情况下，属性是可选的。如需规定属性为必选，请使用 "use" 属性：

```
<xs:attribute name="lang" type="xs:string" use="required"/>
```

## 限定/Facets

### 对值的限定

下面的例子定义了带有一个限定且名为 "age" 的元素。age 的值不能低于 0 或者高于 120：

```
<xs:element name="age">

<xs:simpleType>
  <xs:restriction base="xs:integer">
    <xs:minInclusive value="0"/>
    <xs:maxInclusive value="120"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

### 对一组值的限定

如需把 XML 元素的内容限制为一组可接受的值，我们要使用枚举约束（enumeration constraint）。

下面的例子定义了带有一个限定的名为 "car" 的元素。可接受的值只有：Audi, Golf, BMW：

```
<xs:element name="car">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:enumeration value="Audi"/>
    <xs:enumeration value="Golf"/>
    <xs:enumeration value="BMW"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

上面的例子也可以被写为：

```
<xs:element name="car" type="carType"/>

<xs:simpleType name="carType">
  <xs:restriction base="xs:string">
    <xs:enumeration value="Audi"/>
    <xs:enumeration value="Golf"/>
    <xs:enumeration value="BMW"/>
  </xs:restriction>
</xs:simpleType>
```

**注释：**在这种情况下，类型 "carType" 可被其他元素使用，因为它不是 "car" 元素的组成部分。

### 对一系列值的限定

如需把 XML 元素的内容限制定义为一系列可使用的数字或字母，我们要使用模式约束（pattern constraint）。

下一个例子也定义了带有一个限定的名为 "initials" 的元素。可接受的值是大写或小写字母 a - z 其中的三个：

```
<xs:element name="initials">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="[a-zA-Z][a-zA-Z][a-zA-Z]"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

下一个例子定义了带有一个限定的名为 "choice 的元素。可接受的值是字母 x, y 或 z 中的一个：

```
<xs:element name="choice">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="[xyz]"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

### 对一系列值的其他限定

下面的例子定义了带有一个限定的名为 "letter" 的元素。可接受的值是 a - z 中零个或多个字母：

```
<xs:element name="letter">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="([a-z])*"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

下面的例子定义了带有一个限定的名为 "letter" 的元素。可接受的值是一对或多对字母，每对字母由一个小写字母后跟一个大写字母组成。举个例子，"sToP"将会通过这种模式的验证，但是 "Stop"、"STOP" 或者 "stop" 无法通过验证：

```
<xs:element name="letter">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="([a-z][A-Z])+"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

下面的例子定义了带有一个限定的名为 "gender" 的元素。可接受的值是 male 或者 female：

```
<xs:element name="gender">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="male|female"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

下面的例子定义了带有一个限定的名为 "password" 的元素。可接受的值是由 8 个字符组成的一行字符，这些字符必须是大写或小写字母 a - z 亦或数字 0 - 9：

```
<xs:element name="password">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:pattern value="[a-zA-Z0-9]{8}"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

### 对空白字符的限定

如需规定对空白字符（whitespace characters）的处理方式，我们需要使用 whiteSpace 限定。

下面的例子定义了带有一个限定的名为 "address" 的元素。这个 whiteSpace 限定被设置为 "preserve"，这意味着 XML 处理器不会移除任何空白字符：

```
<xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="preserve"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

这个例子也定义了带有一个限定的名为 "address" 的元素。这个 whiteSpace 限定被设置为 "replace"，这意味着 XML 处理器将移除所有空白字符（换行、回车、空格以及制表符）：

```
<xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="replace"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

这个例子也定义了带有一个限定的名为 "address" 的元素。这个 whiteSpace 限定被设置为 "collapse"，这意味着 XML 处理器将移除所有空白字符（换行、回车、空格以及制表符会被替换为空格，开头和结尾的空格会被移除，而多个连续的空格会被缩减为一个单一的空格）：

```
<xs:element name="address">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:whiteSpace value="collapse"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

### 对长度的限定

如需限制元素中值的长度，我们需要使用 length、maxLength 以及 minLength 限定。

本例定义了带有一个限定且名为 "password" 的元素。其值必须精确到 8 个字符：

```
<xs:element name="password">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:length value="8"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

这个例子也定义了带有一个限定的名为 "password" 的元素。其值最小为 5 个字符，最大为 8 个字符：

```
<xs:element name="password">

<xs:simpleType>
  <xs:restriction base="xs:string">
    <xs:minLength value="5"/>
    <xs:maxLength value="8"/>
  </xs:restriction>
</xs:simpleType>

</xs:element>
```

### 数据类型的限定

|限定|描述|
|:--|:--|
|enumeration|定义可接受值的一个列表|
|fractionDigits|定义所允许的最大的小数位数。必须大于等于0。|
|length|定义所允许的字符或者列表项目的精确数目。必须大于或等于0。|
|maxExclusive|定义数值的上限。所允许的值必须小于此值。|
|maxInclusive|定义数值的上限。所允许的值必须小于或等于此值。|
|maxLength|定义所允许的字符或者列表项目的最大数目。必须大于或等于0。|
|minExclusive|定义数值的下限。所允许的值必需大于此值。|
|minInclusive|定义数值的下限。所允许的值必需大于或等于此值。|
|minLength|定义所允许的字符或者列表项目的最小数目。必须大于或等于0。|
|pattern|定义可接受的字符的精确序列。|
|totalDigits|定义所允许的阿拉伯数字的精确位数。必须大于0。|
|whiteSpace|定义空白字符（换行、回车、空格以及制表符）的处理方式。|

# 复杂类型

## 元素

四种类型的复合元素：

- 空元素
- 包含其他元素的元素
- 仅包含文本的元素
- 包含元素和文本的元素

**注释：**上述元素均可包含属性！

### 例子

```
<employee>
<firstname>John</firstname>
<lastname>Smith</lastname>
</employee>
```

的XML Schema可以写成：

```
<xs:element name="employee">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

或者：

```
<xs:element name="employee" type="personinfo"/>

<xs:complexType name="personinfo">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
  </xs:sequence>
</xs:complexType>
```

也可以在已有的复合元素之上以某个复合元素为基础，然后添加一些元素，就像这样：

```
<xs:element name="employee" type="fullpersoninfo"/>

<xs:complexType name="personinfo">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
  </xs:sequence>
</xs:complexType>

<xs:complexType name="fullpersoninfo">
  <xs:complexContent>
    <xs:extension base="personinfo">
      <xs:sequence>
        <xs:element name="address" type="xs:string"/>
        <xs:element name="city" type="xs:string"/>
        <xs:element name="country" type="xs:string"/>
      </xs:sequence>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
```

## 空元素

一个空的 XML 元素：

```
<product prodid="1345" />
```

上面的 "product" 元素根本没有内容。为了定义无内容的类型，我们就必须声明一个在其内容中只能包含元素的类型，但是实际上我们并不会声明任何元素，比如这样：

```
<xs:element name="product">
  <xs:complexType>
    <xs:complexContent>
      <xs:restriction base="xs:integer">
        <xs:attribute name="prodid" type="xs:positiveInteger"/>
      </xs:restriction>
    </xs:complexContent>
  </xs:complexType>
</xs:element>
```

或者：

```
<xs:element name="product">
  <xs:complexType>
    <xs:attribute name="prodid" type="xs:positiveInteger"/>
  </xs:complexType>
</xs:element>
```

or：

```
<xs:element name="product" type="prodtype"/>

<xs:complexType name="prodtype">
  <xs:attribute name="prodid" type="xs:positiveInteger"/>
</xs:complexType>
```

## 仅含元素

XML 元素，"person"，仅包含其他的元素：

```
<person>
<firstname>John</firstname>
<lastname>Smith</lastname>
</person>
```

可在 schema 中这样定义 "person" 元素：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

或者：

```
<xs:element name="person" type="persontype"/>

<xs:complexType name="persontype">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
  </xs:sequence>
</xs:complexType>
```

## 仅含文本

此类型仅包含简易的内容（文本和属性），因此我们要向此内容添加 simpleContent 元素。当使用简易内容时，我们就必须在 simpleContent 元素内定义扩展或限定，就像这样：

```
<xs:element name="某个名称">
  <xs:complexType>
    <xs:simpleContent>
      <xs:extension base="basetype">
        ....
        ....
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
</xs:element>
```

或者：

```
<xs:element name="某个名称">
  <xs:complexType>
    <xs:simpleContent>
      <xs:restriction base="basetype">
        ....
        ....
      </xs:restriction>
    </xs:simpleContent>
  </xs:complexType>
</xs:element>
```

### 例子

```
<shoesize country="france">35</shoesize>
```

下面这个例子声明了一个复合类型，其内容被定义为整数值，并且 "shoesize" 元素含有名为 "country" 的属性：

```
<xs:element name="shoesize">
  <xs:complexType>
    <xs:simpleContent>
      <xs:extension base="xs:integer">
        <xs:attribute name="country" type="xs:string" />
      </xs:extension>
    </xs:simpleContent>
  </xs:complexType>
</xs:element>
```

我们也可为 complexType 元素设定一个名称，并让 "shoesize" 元素的 type 属性来引用此名称（通过使用此方法，若干元素均可引用相同的复合类型）：

```
<xs:element name="shoesize" type="shoetype"/>

<xs:complexType name="shoetype">
  <xs:simpleContent>
    <xs:extension base="xs:integer">
      <xs:attribute name="country" type="xs:string" />
    </xs:extension>
  </xs:simpleContent>
</xs:complexType>
```

## 混合内容

带有混合内容的复合类型
XML 元素，"letter"，含有文本以及其他元素：

```
<letter>
Dear Mr.<name>John Smith</name>.
Your order <orderid>1032</orderid>
will be shipped on <shipdate>2001-07-13</shipdate>.
</letter>
```

下面这个 schema 声明了这个 "letter" 元素：

```
<xs:element name="letter">
  <xs:complexType mixed="true">
    <xs:sequence>
      <xs:element name="name" type="xs:string"/>
      <xs:element name="orderid" type="xs:positiveInteger"/>
      <xs:element name="shipdate" type="xs:date"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

**注释：**为了使字符数据可以出现在 "letter" 的子元素之间，mixed 属性必须被设置为 "true"。<xs:sequence\> 标签 (name、orderid 以及 shipdate ) 意味着被定义的元素必须依次出现在 "letter" 元素内部。

或者：

```
<xs:element name="letter" type="lettertype"/>

<xs:complexType name="lettertype" mixed="true">
  <xs:sequence>
    <xs:element name="name" type="xs:string"/>
    <xs:element name="orderid" type="xs:positiveInteger"/>
    <xs:element name="shipdate" type="xs:date"/>
  </xs:sequence>
</xs:complexType>
```

## 指示器

七种指示器：

Order 指示器：

- All
- Choice
- Sequence

Occurrence 指示器：

- maxOccurs
- minOccurs

Group 指示器：

- Group name
- attributeGroup name

### ALL

<all\> 指示器规定子元素可以按照任意顺序出现，且每个子元素必须只出现一次.

当使用 <all\> 指示器时，你可以把 <minOccurs\> 设置为 0 或者 1，而只能把 <maxOccurs\> 指示器设置为 1

```
<xs:element name="person">
  <xs:complexType>
    <xs:all>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
    </xs:all>
  </xs:complexType>
</xs:element>
```

### Choice

<choice\> 指示器规定可出现某个子元素或者可出现另外一个子元素（非此即彼）,如需设置子元素出现任意次数，可将 <maxOccurs\> 设置为 unbounded（无限次）：

```
<xs:element name="person">
  <xs:complexType>
    <xs:choice>
      <xs:element name="employee" type="employee"/>
      <xs:element name="member" type="member"/>
    </xs:choice>
  </xs:complexType>
</xs:element>
```

### Sequence

<sequence\> 规定子元素必须按照特定的顺序出现：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### maxOccurs

Occurrence 指示器用于定义某个元素出现的频率。对于所有的 "Order" 和 "Group" 指示器（any、all、choice、sequence、group name 以及 group reference），其中的 maxOccurs 以及 minOccurs 的默认值均为 1。

<maxOccurs\>指示器可规定某个元素可出现的最大次数,如需使某个元素的出现次数不受限制，可以使用 maxOccurs="unbounded" 这个声明：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="full_name" type="xs:string"/>
      <xs:element name="child_name" type="xs:string" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### minOccurs

<minOccurs\>指示器可规定某个元素能够出现的最小次数：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="full_name" type="xs:string"/>
      <xs:element name="child_name" type="xs:string"
      maxOccurs="10" minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### Group name

元素组通过 group 声明进行定义：

```
<xs:group name="组名称">
  ...
</xs:group>
```

您必须在 group 声明内部定义一个 all、choice 或者 sequence 元素。下面这个例子定义了名为 "persongroup" 的 group，它定义了必须按照精确的顺序出现的一组元素：

```
<xs:group name="persongroup">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
    <xs:element name="birthday" type="xs:date"/>
  </xs:sequence>
</xs:group>
```

在您把 group 定义完毕以后，就可以在另一个定义中引用它了：

```
<xs:group name="persongroup">
  <xs:sequence>
    <xs:element name="firstname" type="xs:string"/>
    <xs:element name="lastname" type="xs:string"/>
    <xs:element name="birthday" type="xs:date"/>
  </xs:sequence>
</xs:group>

<xs:element name="person" type="personinfo"/>

<xs:complexType name="personinfo">
  <xs:sequence>
    <xs:group ref="persongroup"/>
    <xs:element name="country" type="xs:string"/>
  </xs:sequence>
</xs:complexType>
```

### attributeGroup name

属性组通过 attributeGroup 声明来进行定义：

```
<xs:attributeGroup name="组名称">
  ...
</xs:attributeGroup>
```

下面这个例子定义了名为 "personattrgroup" 的一个属性组：

```
<xs:attributeGroup name="personattrgroup">
  <xs:attribute name="firstname" type="xs:string"/>
  <xs:attribute name="lastname" type="xs:string"/>
  <xs:attribute name="birthday" type="xs:date"/>
</xs:attributeGroup>
```

在您已定义完毕属性组之后，就可以在另一个定义中引用它了，就像这样：

```
<xs:attributeGroup name="personattrgroup">
  <xs:attribute name="firstname" type="xs:string"/>
  <xs:attribute name="lastname" type="xs:string"/>
  <xs:attribute name="birthday" type="xs:date"/>
</xs:attributeGroup>

<xs:element name="person">
  <xs:complexType>
    <xs:attributeGroup ref="personattrgroup"/>
  </xs:complexType>
</xs:element>
```

## <any\>

<any\> 元素使我们有能力通过未被 schema 规定的元素来拓展 XML 文档！
下面这个例子是从名为 "family.xsd" 的 XML schema 中引用的片段。它展示了一个针对 "person" 元素的声明。通过使用 <any\> 元素，我们可以通过任何元素（在 <lastname\> 之后）扩展 "person" 的内容：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
      <xs:any minOccurs="0"/>
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

## <anyAttribute\>

<anyAttribute\> 元素使我们有能力通过未被 schema 规定的属性来扩展 XML 文档！
下面的例子是来自名为 "family.xsd" 的 XML schema 的一个片段。它为我们展示了针对 "person" 元素的一个声明。通过使用 <anyAttribute\> 元素，我们就可以向 "person" 元素添加任意数量的属性：

```
<xs:element name="person">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="firstname" type="xs:string"/>
      <xs:element name="lastname" type="xs:string"/>
    </xs:sequence>
    <xs:anyAttribute/>
  </xs:complexType>
</xs:element>
```

## 元素替换

让我们举例说明：我们的用户来自英国和挪威。我们希望有能力让用户选择在 XML 文档中使用挪威语的元素名称还是英语的元素名称。
为了解决这个问题，我们可以在 XML schema 中定义一个 substitutionGroup。首先，我们声明主元素，然后我们会声明次元素，这些次元素可声明它们能够替换主元素。

```
<xs:element name="name" type="xs:string"/>
<xs:element name="navn" substitutionGroup="name"/>

<xs:complexType name="custinfo">
  <xs:sequence>
    <xs:element ref="name"/>
  </xs:sequence>
</xs:complexType>

<xs:element name="customer" type="custinfo"/>
<xs:element name="kunde" substitutionGroup="customer"/>
```

有效的 XML 文档类似这样（根据上面的 schema）：

```
<customer>
  <name>John Smith</name>
</customer>
```

或类似这样：

```
<kunde>
  <navn>John Smith</navn>
</kunde>
```

如果需要阻止元素替换，可使用 block 属性：

```
<xs:element name="name" type="xs:string" block="substitution"/>
```

请注意，substitutionGroup 中的所有元素（主元素和可替换元素）必须被声明为全局元素，否则就无法工作！

全局元素指 "schema" 元素的直接子元素！本地元素（Local elements）指嵌套在其他元素中的元素。

# 数据类型

## 字符串

### 字符串数据类型（String Data Type）

字符串数据类型可包含字符、换行、回车以及制表符。如果您使用字符串数据类型，XML 处理器就不会更改其中的值。

```
<xs:element name="customer" type="xs:string"/>
```

### 规格化字符串数据类型（NormalizedString Data Type）

规格化字符串数据类型同样可包含字符，但是 XML 处理器会移除折行，回车以及制表符。

下面是一个关于在某个 schema 中规格化字符串数据类型的例子：

```
<xs:element name="customer" type="xs:normalizedString"/>
```

文档中的元素看上去应该类似这样：

```
<customer>John Smith</customer>
```

或者类似这样：

```
<customer>	John Smith	</customer>
```

注释：在上面的例子中，XML 处理器会使用空格替换所有的制表符。

### Token 数据类型（Token Data Type）

Token 数据类型同样可包含字符，但是 XML 处理器会移除换行符、回车、制表符、开头和结尾的空格以及（连续的）空格。

下面是在 schema 中一个有关 token 声明的例子：

```
<xs:element name="customer" type="xs:token"/>
```

文档中的元素看上去应该类似这样：

```
<customer>John Smith</customer>
```

或者类似这样：

```
<customer>	John Smith	</customer>
```

注释：在上面这个例子中，XML 解析器会移除制表符。

### 字符串数据类型

|名称|描述|
|:--|:--|
|ENTITIES| |
|ENTITY| |
|ID|在 XML 中提交 ID 属性的字符串 (仅与 schema 属性一同使用)|
|IDREF|在 XML 中提交 IDREF 属性的字符串(仅与 schema 属性一同使用)|
|IDREFS language|包含合法的语言 id 的字符串|
|Name|包含合法 XML 名称的字符串|
|NCName| |
|NMTOKEN|在 XML 中提交 NMTOKEN 属性的字符串 (仅与 schema 属性一同使用)|
|NMTOKENS| |
|normalizedString|不包含换行符、回车或制表符的字符串|
|QName| |
|string|字符串|
|token|不包含换行符、回车或制表符、开头或结尾空格或者多个连续空格的字符串|

### 对字符串数据类型的限定（Restriction）

可与字符串数据类型一同使用的限定：

- enumeration
- length
- maxLength
- minLength
- pattern (NMTOKENS、IDREFS 以及 ENTITIES 无法使用此约束)
- whiteSpace

## 日期

### 日期数据类型（Date Data Type）

日期使用此格式进行定义："YYYY-MM-DD"

下面是一个有关 schema 中日期声明的例子：

```
<xs:element name="start" type="xs:date"/>
```

文档中的元素看上去应该类似这样：

```
<start>2002-09-24</start>
```

如需规定一个时区，您也可以通过在日期后加一个 "Z" 的方式，使用世界调整时间（UTC time）来输入一个日期 - 比如这样：

```
<start>2002-09-24Z</start>
```

或者也可以通过在日期后添加一个正的或负时间的方法，来规定以世界调整时间为准的偏移量 - 比如这样：

```
<start>2002-09-24-06:00</start>
```

或者：

```
<start>2002-09-24+06:00</start>
```

### 时间数据类型（Time Data Type）

时间使用下面的格式来定义："hh:mm:ss"

下面是一个有关 schema 中时间声明的例子：

```
<xs:element name="start" type="xs:time"/>
```

时区同上

### 日期时间数据类型（DateTime Data Type）

日期时间使用下面的格式进行定义："YYYY-MM-DDThh:mm:ss"

下面是一个有关 schema 中日期时间声明的例子：

```
<xs:element name="startdate" type="xs:dateTime"/>
```

```
<startdate>2002-05-30T09:00:00</startdate>
```

时区同上

### 持续时间数据类型（Duration Data Type）

时间间隔使用下面的格式来规定："PnYnMnDTnHnMnS"，其中：

- P 表示周期(必需)
- nY 表示年数
- nM 表示月数
- nD 表示天数
- T 表示时间部分的起始 （如果您打算规定小时、分钟和秒，则此选项为必需）
- nH 表示小时数
- nM 表示分钟数
- nS 表示秒数

如需规定一个负的持续时间，请在 P 之前输入减号：

```
<period>-P10D</period>
```

### 日期和时间数据类型

|名称|描述|
|:--|:--|
|date|定义一个日期值|
|dateTime|定义一个日期和时间值|
|duration|定义一个时间间隔|
|gDay|定义日期的一个部分 - 天 (DD)|
|gMonth|定义日期的一个部分 - 月 (MM)|
|gMonthDay|定义日期的一个部分 - 月和天 (MM-DD)|
|gYear|定义日期的一个部分 - 年 (YYYY)|
|gYearMonth|定义日期的一个部分 - 年和月 (YYYY-MM)|
|time|定义一个时间值|

### 对日期数据类型的限定（Restriction）

可与日期数据类型一同使用的限定：

- enumeration
- maxExclusive
- maxInclusive
- minExclusive
- minInclusive
- pattern
- whiteSpace

## 数值

### 十进制数据类型

```
<xs:element name="prize" type="xs:decimal"/>
```

您可规定的十进制数字的最大位数是 18 位。

### 整数数据类型

```
<xs:element name="prize" type="xs:integer"/>
```

### 数值数据类型

|名字|秒数|
|:--|:--|
|byte|有正负的 8 位整数|
|decimal|十进制数|
|int|有正负的 32 位整数|
|integer|整数值|
|long|有正负的 64 位整数|
|negativeInteger|仅包含负值的整数 ( .., -2, -1.)|
|nonNegativeInteger|仅包含非负值的整数 (0, 1, 2, ..)|
|nonPositiveInteger|仅包含非正值的整数 (.., -2, -1, 0)|
|positiveInteger|仅包含正值的整数 (1, 2, ..)|
|short|有正负的 16 位整数|
|unsignedLong|无正负的 64 位整数|
|unsignedInt|无正负的 32 位整数|
|unsignedShort|无正负的 16 位整数|
|unsignedByte|无正负的 8 位整数|

### 对数值数据类型的限定（Restriction）

可与数值数据类型一同使用的限定：

- enumeration
- fractionDigits
- maxExclusive
- maxInclusive
- minExclusive
- minInclusive
- pattern
- totalDigits
- whiteSpace

## 杂项

其他杂项数据类型包括逻辑、base64Binary、十六进制、浮点、双精度、anyURI、anyURI 以及 NOTATION。

### 逻辑数据类型（Boolean Data Type）

逻辑数据性用于规定 true 或 false 值。

```
<xs:attribute name="disabled" type="xs:boolean"/>
```

合法的布尔值是 true、false、1（表示 true） 以及 0（表示 false）。

### 二进制数据类型（Binary Data Types）

二进制数据类型用于表达二进制形式的数据。

我们可使用两种二进制数据类型：

- base64Binary (Base64 编码的二进制数据)
- hexBinary (十六进制编码的二进制数据)

下面是一个关于某个 scheme 中 hexBinary 声明的例子：

```
<xs:element name="blobsrc" type="xs:hexBinary"/>
```

### AnyURI 数据类型（AnyURI Data Type）

anyURI 数据类型用于规定 URI。

下面是一个关于某个 scheme 中 anyURI 声明的例子：

```
<xs:attribute name="src" type="xs:anyURI"/>
```

文档中的元素看上去应该类似这样：

```
<pic src="http://www.w3school.com.cn/images/smiley.gif" />
```

注释：假如某个 URI 含有空格，请用 %20 替换它们。

### 杂项数据类型（Miscellaneous Data Types）

- anyURI
- base64Binary
- boolean
- double
- float
- hexBinary
- NOTATION
- QName

### 对杂项数据类型的限定（Restriction）

可与杂项数据类型一同使用的限定：

- enumeration (布尔数据类型无法使用此约束*)
- length (布尔数据类型无法使用此约束)
- maxLength (布尔数据类型无法使用此约束)
- minLength (布尔数据类型无法使用此约束)
- pattern
- whiteSpace