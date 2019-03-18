---
title: owl本体语言学习笔记
date: 2019-03-18 20:49:42
tags: [owl, 本体, 学习笔记]
---

# OWL简介

OWL(Web Ontology Language)是W3C开发的一种网络本体语言，用于对本体进行语义描述。OWL是针对各方面的需求在DAML+OIL的基础上进行改进而开发的，它一方面保持了对DAML+oIL／RDFs的兼容性，另一方面又保证了更加强大的语义表达能力，同时还要保证描述逻辑(DL，Description Logic)的可判定推理。W3C的设计人员针对各类特征的需求制定了三种相应的OWL的子语言，即OWL Lite、OWL DL和OWL Full，三种子语言的表达能力递增。

1. OWL Lite是表达能力最弱的子语言。它是傩乙DL的一个子集，但是通过降低OWL DL中的公理约束，保证了迅速高效的推理。它支持基数约束，但基数值只能为O或l。因为0WL Lite表达能力较弱，为其开发支持工具要比其他两个子语言容易一些。OWL Lite用于提供给那些仅需要一个分类层次和简单约束的用户。
2. OWL DL(Description Logic，描述逻辑)将可判定推理能力和较强表达能力作为首要目标，而忽略了对RDFS的兼容性。0WL DL包括了OWL语言的所有语言成分，但使用时必须符合一定的约束，受到一定的限制。OWL DL提供了描述逻辑的推理功能，描述逻辑是OWL的形式化基础。
3. OWL Full包含OWL的全部语言成分并取消了OWL DL中的限制，它将RDFS扩展为一个完备的本体语言，支持那些不需要可计算性保证(no computational guarantees)但需要最强表达能力和完全自由的RDFS用户。在OWL Full中，一个类可以看成是个体的集合，也可以看成是一个个体。由于OWL Full取消了基数限制中对可传递性质的约束，因此不能保证可判定推理。

# OWL本体的组成

## 个体（individual）

个体代表领域中我们感兴趣的对象，OWL不使用唯一命名假设，即两个不同的名称可以对应一个个体（例如：“伊丽莎白女王”和“伊丽莎白温莎”是指同一个人）。在OWL中，必须明确表示个体之间是否相同，否则它们的关系是不明确的。

个体（individual）有时也被称作实例（Instance）。

## 属性（Property）

属性是个体之间的二元关系。在描述逻辑中，它们就是角色（Role）的概念。

按照属性的表意及性质可以分为以下四类属性：

- 函数属性(Functional Property)——通过这个属性只能连接一个个体，如hasBirthMother
- 反函数属性(Inverse Functional Property)——即这个属性的反属性是函数属性，也就是对于一个给定的个体，只有最多一个个体能通过该属性连接那个个体，如isBirthMotherOf
- 传递属性(Transitive Property)——这个属性是可以传递的，如你的祖先的祖先也是你的祖先，hasAncestor
- 对称属性(Symmetric Property)——即这个属性是对称的，一个属性是对称的那么它就不能是函数属性。如你是你的兄弟的兄弟，hasSibling

按照属性的链接对象不同可以分为以下三类：

- 对象属性(Object Property)——连接两个个体。
- 数据类型属性(Datatype Property)——连接个体和XML Schema数据类型值或rdf literal,该属性不能为传递的，对称的，反函数的。
- 标注属性 (Annotation Property)——用来对类，属性，个体和本体添加信息(元数据)。OWL-DL对标注属性作出了如下限制：(1)标注属性的filler只能为,literal或URI或个体。(2)标注属性没有子属性，也不能为其它属性的子属性，而且不能使用domain和range。

## 类（class）

表示一些个体的集合，它使用数学的方法描述出该类中成员必须具有的条件。概念（concept）这个词有时被用来代替类，实际上，类是概念的一个具体表现。

# OWL中本体的结构

## 命名空间

在使用一组术语之前，需要精确地指出哪些具体的词汇表将会用到。一个典型的OWL本体以命名空间声明开始，这些命名空间写到<rdf:RDF\>标签中。
	
属性值是不具有命名空间的，在OWL里可以写出它们的完整URI。完整的URI中可以利用实体定义来简略。
如：

```
<!DOCTYPE rdf:RDF [
     <!ENTITY vin  "http://www.w3.org/TR/2004/REC-owl-guide-20040210/wine#" >
     <!ENTITY food "http://www.w3.org/TR/2004/REC-owl-guide-20040210/food#" > ]>
```

在声明这些实体后，我们可以将“&vin;merlot”作为`http://www.w3.org/TR/2004/REC -owl-guide-20040210/wine#merlot`的简写。

## 本体头部

在owl：Ontology标签中给出本体的声明。这些标签支持一些重要的常务工作比如注释、版本控制以及其他本体的嵌入等。

owl:Ontology元素是用来收集关于当前文档的OWL元数据的。

rdf:about属性为本体提供一个名称或引用。

rdfs:comment提供了显然必须的为本体添加注解的能力。

owl:priorVersion是一个为用于本体的版本控制系统提供相关信息（hook）的标准标签。本体的版本控制将在后面作进一步讨论。

owl:imports提供了一种嵌入机制。owl:imports接受一个用rdf:resource属性标识的参数。

## 数据集成与隐私

不同的个体成员可能表示同一个体，owl：sameAs表达等价的能力。

# 基本元素

## 简单的个体和类

外延：我们称由属于某个类的个体所构成的集合为该类的外延（extension）。

本体：为了进行相关个体的推理。

### 简单的具名类

一个领域中最基本的概念对应各个分类层次树的根。

。。。。国际化资源标识符（IRI）。。。。。统一资源标识符（URI）。。。。

rdf:ID="Region" 被用于引入一个名称（作为定义的一部分）
在这一文档中，我们现在可以用#Region来引用Region类，例如 rdf:resource="#Region"

rdfs:subClassOf是用于类的基本分类构造符，次关系是可传递的
一个类的定义由两部分组成：引入或引用一个名称，以及一个限制表。

### 个体

```
<owl:Thing rdf:ID="CentralCoastRegion" /> 
<owl:Thing rdf:about="#CentralCoastRegion"> 
<rdf:type rdf:resource="#Region"/> 
/*表示个体，type是一个rdf属性，用于关联一个个体和它所属的类*/
  	
</owl:Thing>
```

或者使用`<Region rdf:ID="CentralCoastRegion" />` 语句来表示个体

Web本体被设计成为分布式的，我们可以通过导入和补充已有的本体来创建衍生的本体。

### 使用方面的考虑

一个类仅是一个名称和一些描述某集合内个体的属性；而个体是该集合的成员。因此，类应自然地对应于与某论域中的事物的出现集合，而个体应对应于可被归入这些类的实际的实体。
	
子类：类的子集合
	
实例：表示一个单一的个体

一个本体的开发应坚定地由它的预定用途所驱动。这些问题也存在于OWL Full和OWL DL之间的一个重要区别。OWL Full允许将类（class）用作实例（instance），而OWL DL不允许。

## 简单属性

一个属性是一个二元关系，有两种类型的属性：

数据类型属性（datatype properties）：类实例与RDF文字或XML Schema数据类型间的关系。

对象属性（object properties）：两个类的实例间的关系。

### 定义属性

```
<owl:ObjectProperty rdf:ID="madeFromGrape"> 
    <rdfs:domain rdf:resource="#Wine"/> /*表示定义域*/
    <rdfs:range rdf:resource="#WineGrape"/> /*表示值域*/
</owl:ObjectProperty>
```

在OWL中，一个值域可被用来推断一个类型

```
<owl:Thing rdf:ID="LindemansBin65Chardonnay">
    <madeFromGrape rdf:resource="#ChardonnayGrape" />
</owl:Thing>
```

可以推断出，LindemansBin65Chardonnay为一种葡萄酒，因为其定义域为wine

可以定义子属性，属性是传递的，例如X为Y的子属性，如果具有属性X，则必然同时具有属性Y。

### 属性和数据类型

数据类型属性：将个体关联到数据（值域为：RDF文字或XML Schema数据类型）

```
<owl:Class rdf:ID="VintageYear" />
<owl:DatatypeProperty rdf:ID="yearValue">
<rdfs:domain rdf:resource="#VintageYear" />
    <rdfs:range  rdf:resource="&xsd;positiveInteger"/>
</owl:DatatypeProperty> 
```

yearValue属性将VintageYears与一个整数值相关联。

### 个体的属性

```
<Region rdf:ID="SantaCruzMountainsRegion">
    <locatedIn rdf:resource="#CaliforniaRegion" />
</Region>
<Winery rdf:ID="SantaCruzMountainVineyard" />
<CabernetSauvignon   rdf:ID="SantaCruzMountainVineyardCabernetSauvignon" >
    <locatedIn   rdf:resource="#SantaCruzMountainsRegion"/>  
    <hasMaker    rdf:resource="#SantaCruzMountainVineyard" />   
</CabernetSauvignon>
```

## 属性的特性

- 传递属性： P(x，y)，P(y，z)  P(x，z)
- 对称属性： p(x，y)当且仅当P(y, x)【注意是同一个关系】
- 函数属性： P(x,y) 与P(x,z) 蕴含 y = z，即对应值的唯一性
- 逆属性 （inverseOf）：P1(x,y) 当且仅当P2(y,x)【注意是不同关系】
- 反函数属性 （InverseFunctional）：P(y,x) 与 P(z,x) 蕴含 y = z；

InverseFunctional意味着属性的值域中的元素为定义域中的每个元素提供了一个唯一的标识。

## 属性限制

### 两个属性限制机制

- allValuesFrom
- someValuesFrom

它们都是是局部的（local），仅仅在包含它们的类的定义中起作用。

owl:allValuesFrom属性限制要求：对于每一个有指定属性实例的类实例，该属性的值必须是由owl:allValuesFrom从句指定的类的成员。

owl:someValuesFrom限制与之相似。

**例子：**Wine的制造商必须是Winery。allValuesFrom限制仅仅应用在Wine的hasMaker 属性上。Cheese的制造商并不受这一局部限制的约束。（代码如下）

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318202408.png)

|关系|含意|
|:--|:--|
|allValuesFrom|对于所有的葡萄酒，如果它们有制造商，那么所有的制造商都是酿酒厂|
|someValuesFrom|对于所有的葡萄酒，它们中至少有一个的制造商是酿酒厂|

### 基数限制

owl:cardinality：这一约束允许对一个关系中的元素数目作出精确的限制。

例如，我们可以将Vintage标识为恰好含有一个VintageYear的类。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318202608.png)

值域限制在0和1的基数表达式(Cardinality expressions)是OWL Lite的一部分。这使得用户能够表示“至少一个”，“不超过一个”，和“恰好一个”这几种意思。OWL DL中还允许使用除0与1以外的正整数值。owl:maxCardinality能够用来指定一个上界。owl:minCardinality能够用来指定一个下界。使用二者的组合就能够将一个属性的基数限制为一个数值区间。

### hasValue

hasValue 使得我们能够根据“特定的”属性值的存在来标识类。因此，一个个体只要至少有“一个”属性值等于hasValue的资源，这一个体就是该类的成员。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318202659.png)

如果是Burgundy酒，那就都是干(dry)的酒。也即，它们的hasSugar属性必须至少有一个是值等于Dry（干的）。
【我的理解是，每个Burgundy都要有一个干的（Dry）属性，以此来标识该酒是干酒】

# 本体映射

用于实现本体的共享。

## 类和属性之间的等价关系（equivalentClass，equivalentProperty）

属性owl:equivalentClass被用来表示两个类有着完全相同的实例。但我们要注意，在OWL DL中，类仅仅代表着个体的集合而不是个体本身。然而在OWL FULL中，我们能够使用owl:sameAs来表示两个类在各方面均完全一致。

类似的，我们可以通过使用owl:equivalentProperty属性声明表达属性的等同。

## 个体间的同一性

SameAs：描述个体之间相同的机制与描述类之间的相同机制类似，仅仅只要两个个体的声明形成一致的就可以了。

假如hasMaker是一个函数型属性，那么下面的例子就不一定会产生冲突。

```
<owl:Thing rdf:about="#BancroftChardonnay">
    <hasMaker rdf:resource="#Bancroft" />
    <hasMaker rdf:resource="#Beringer" />
</owl:Thing>
```

除非和我们本体中的其他信息发生冲突，不然的话这样的描述是没有冲突的，他说明Bancroft和Beringer是相同的个体。

要清楚，修饰（或引用）两个类用sameAs还是用equivalentClass效果是不同的。用sameAs的时候，把一个类解释为一个个体，就像在OWL Full中一样，这有利于对本体进行分类。在OWL Full中，sameAs可以用来引用两个东西，如一个类和一个个体、一个类和一个属性等等，无论什么情况，都将被解释为个体。

## 不同的个体

这一机制提供了与sameAs相反的效果。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318203431.png)

说明了三个值互不相同。如果我们没有用 differentFrom元素来申明既干又甜的葡萄酒，这意味着“干葡萄酒”和“甜葡萄酒”是相同的。但是我们从上面申明的元素来推断，这又是矛盾的。还有一种更便利的定义相互不同个体的机制，如下

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318203844.png)

要注意，owl:distinctMembers属性声明只能和owl:AllDifferent属性声明一起结合使用。

# 复杂类

用于创建类的表达式。OWL支持基本的集合操作，即并，交和补运算。它们分别被命名为owl:unionOf,owl:intersectionOf,和owl:complementOf.此外，类还可以是枚举的。类的外延可以使用oneOf构造子来显示的声明。同时，我们也可以声明类的外延必须是互不相交的。

注意：OWL类外延是由个体组成的集合，二这些个体都是类的成员。

## 集合运算符

### 交运算

```
<owl:Class rdf:ID="WhiteWine">
   <owl:intersectionOf rdf:parseType="Collection"> /*这是必须的，因为必须对集合操作*/
     <owl:Class rdf:about="#Wine" />
     <owl:Restriction>
       <owl:onProperty rdf:resource="#hasColor" />
       <owl:hasValue rdf:resource="#White" />
     </owl:Restriction>
   </owl:intersectionOf>
</owl:Class>
```

这个例子表示，白葡萄酒就是葡萄酒和白色物体的相交的集合。如果不这么表示，计算机只知道，白葡萄酒有白色的属性；却不知道，所有白色的葡萄酒是白葡萄酒

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318203750.png)

WhiteBurgundy类恰好是白葡萄酒和Burgundies的交集。依次，Burgundies生产在法国一个叫做Bourgogne的地方并且它是干葡萄酒（dry wine）。因此，所有满足这些标准的葡萄酒个体都是WhiteBurgundy类的外延的一部分。

### 并运算

表示两个集合的∪。使用方法同上，将intersectionOf改成unionOf。

### 补运算

就是表示差集，complementOf典型的用法是与其它集合运算符联合使用，如下

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204251.png)

上面的例子定义了一个NonFrenchWine类，它是Wine类与所有不位于法国的事物的集合的交集。

## 枚举类（one of）

以直接枚举的方式描述类的成员。特别的，这个定义完整的描述了类的外延（类的范围？），因此任何其他个体都不能声明为属于这个类。如下：

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204336.png)

这段代码说明，WineColor只包含三种，white rose和red，任何其他的颜色都不是winecolor类的实例

oneOf结构的每一个元素都必须是一个有效声明的个体。一个个体必须属于某个类。在上面的例子中，每一个个体都是通过名字来引用的。我们使用owl:Thing简单地进行引用，尽管这有点多余（因为每个个体都属于owl:Thing）。另外，我们也可以根据具体类型WineColor来引用集合中的元素：

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204408.png)

另外，较复杂的个体描述同样也可以是oneOf结构的有效元素，例如:

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204538.png)

## 不相交类（disjointWith）

使用owl:disjointWith构造子可以表达一组类是不相交的。它保证了属于某一个类的个体不能同时又是另一个指定类的实例。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204613.png)

Pasta例子声明了多个不相交类。注意它只声明了Pasta与其它所有类是不相交的。例如，它并没有保证Meat和Fruit是不相交的。为了声明一组类是互不相交的，我们必须对每两个类都使用owl:disjointWith来声明。

在下面的例子中，我们定义了Fruit是SweetFruit和NonSweetFruit的并集。而且我们知道这些子类恰好将Fruit划分成了连个截然不同的子类，因为它们是互不相交的。随着互不相交的类的增加，不相交的声明的数目也会相应的增加到n的2次方。然而，在我们已知的用例中，n通常比较小。

![](https://raw.githubusercontent.com/imonce/imgs/master/20190318204656.png)

# 本体版本控制

本体和软件一样需要维护，因此它们将随着时间的推移而改变。在一个owl:Ontology元素（如上面讨论的`http://www.w3.org/TR/2004/REC-owl-guide-20040210/#OntologyHeaders`） 内，链接到一个以前定义的本体版本是可能的。属性owl:priorVersion被用来提供这种链接，并能用它跟踪一个本体的版本历史。
	
本体版本可能彼此互不兼容，例如，一个本体以前的版本可能包含与现在版本中的陈述相矛盾的陈述。在一个owl:Ontology元素中，我们使用owl:backwardCompatibleWith和owl:incompatibleWith这些属性来指出本体版本是兼容还是不兼容以前的版本。如果没有进行owl:backwardCompatibleWith声明，那么我们假定就不存在兼容性。除了上面讲到的两个属性，还有一个属性owl:versionInfo适用与版本控制系统，它提供了一些相关信息（hook）。和前面三个属性相反的是，owl:versionInfo的客体是一个文字值（literal），这一属性除了可以用来注释本体之外还可以用来注释类和属性。

# Reference

1. [OWL本体语言中OWL Lite、OWL DL、OWL Full理解](https://blog.csdn.net/qq_38842357/article/details/80706872)
2. [owl本体语言学习笔记（一）](https://sophieling.iteye.com/blog/836545)
3. [owl本体语言学习笔记（二）](https://sophieling.iteye.com/blog/836548)