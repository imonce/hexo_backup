---
title: '3小时精通lxml.etree:Python中xml的读取、解析、生成和查找'
date: 2019-10-21 17:04:41
tags: [python, python3, lxml, etree]
category: [Learn X in Y minutes, Learn lxml.etree in Y minutes]
---

本文主要参考官方文档[https://lxml.de/tutorial.html](https://lxml.de/tutorial.html)整理，如有错误欢迎指出。


```python
from lxml import etree
from copy import deepcopy
```


```python
# 添加Element默认添加为根节点
root = etree.Element("root")
# 通过append添加子节点
root.append(etree.Element("child1"))
# 通过SubElement添加子节点
child2 = etree.SubElement(root, "child2")
child3 = etree.SubElement(root, "child3")
print(etree.tostring(root))
print(etree.tostring(root, pretty_print=True))
# 默认encoding是ASCII
print(etree.tostring(root, encoding='iso-8859-1'))
# 如果要把byte转成str输出的话，可以用下边的语句
print(str(etree.tostring(root, pretty_print=True),encoding='utf-8'))
```

    b'<root><child1/><child2/><child3/></root>'
    b'<root>\n  <child1/>\n  <child2/>\n  <child3/>\n</root>\n'
    b"<?xml version='1.0' encoding='iso-8859-1'?>\n<root><child1/><child2/><child3/></root>"
    <root>
      <child1/>
      <child2/>
      <child3/>
    </root>
    



```python
# 每个元素都是一个list
child1 = root[0]
# .tag 可以取出元素的标签
print(child1.tag, len(root))
# 大部分list方法都可以使用在element上
root.insert(0, etree.Element("child0"))
print(root[0].tag)
```

    child1 3
    child0



```python
# 可以用list函数取出子元素的list
children = list(root)
print(root)
print(children)
```

    <Element root at 0x107b03888>
    [<Element child0 at 0x107b034c8>, <Element child1 at 0x107b03448>, <Element child2 at 0x107b038c8>, <Element child3 at 0x107b03908>]



```python
# 通过长度检查element是否为叶子节点
if len(root):
    print('root has children')
if len(child1):
    print('child1 has children')
else:
    print('child1 has no child')
```

    root has children
    child1 has no child



```python
# 不同于普通list，赋值可能会造成元素移动
print(etree.tostring(root))
root[0] = root[-1]
print(etree.tostring(root))

# 这是普通list
l = [0,1,2,3]
print(l)
l[0] = l[-1]
print(l)
```

    b'<root><child0/><child1/><child2/><child3/></root>'
    b'<root><child3/><child1/><child2/></root>'
    [0, 1, 2, 3]
    [3, 1, 2, 3]



```python
# 要拷贝节点，需要调用deepcopy
newroot = etree.Element('newroot')
newroot.append(deepcopy(root[1]))
print(etree.tostring(root))
print(etree.tostring(newroot))
```

    b'<root><child3/><child1/><child2/></root>'
    b'<newroot><child1/></newroot>'



```python
# etree元素自带方法可以找到对应的父节点、前一个节点、后一个节点
print(root is child1.getparent())
print(root[0] is root[1].getprevious())
print(root[1] is root[0].getnext())
```

    True
    True
    True



```python
# 属性以字典方式存储
root = etree.Element("root", interesting="totally")
print(etree.tostring(root))
# 通过get方法获取属性值
print(root.get("interesting"))
print(root.get("hello"))
# 通过set方法添加属性
root.set("hello", "Huhu")
print(etree.tostring(root))
```

    b'<root interesting="totally"/>'
    totally
    None
    b'<root interesting="totally" hello="Huhu"/>'



```python
# 字典的相关方法也可以直接使用
print(root.items(), root.keys(), root.values())

# 通过 attrib 可以直接取出属性进行操作
attributes = root.attrib
print(attributes.get("no-such-attribute"))
None
attributes["hello"] = "Guten Tag"
print(attributes["hello"])
# root中的属性一并修改
print(root.get("hello"))

# 也可以转化为纯正的dict
d = dict(root.attrib)
print(d.items())
```

    [('interesting', 'totally'), ('hello', 'Huhu')] ['interesting', 'hello'] ['totally', 'Huhu']
    None
    Guten Tag
    Guten Tag
    dict_items([('interesting', 'totally'), ('hello', 'Guten Tag')])



```python
# 元素中包含文本
root = etree.Element("root")
root.text = "Hello World"
print(root.text)
print(etree.tostring(root))
```

    Hello World
    b'<root>Hello World</root>'



```python
# 如何添加特殊元素类似于<br/>
html = etree.Element("html")
body = etree.SubElement(html, "body")
body.text = "TEXT"

print(etree.tostring(html))

br = etree.SubElement(body, "br")
print(etree.tostring(html))

br.tail = "TAIL"
print(etree.tostring(html))
body.text = "HEAD"
print(etree.tostring(html))
```

    b'<html><body>TEXT</body></html>'
    b'<html><body>TEXT<br/></body></html>'
    b'<html><body>TEXT<br/>TAIL</body></html>'
    b'<html><body>HEAD<br/>TAIL</body></html>'



```python
# tail其实属于前边元素的一部分
print(etree.tostring(br))
print(etree.tostring(br, with_tail=False))
print(etree.tostring(html, method="text"))
```

    b'<br/>TAIL'
    b'<br/>'
    b'HEADTAIL'



```python
# xpath方法也可以使用
print(html.xpath("string()"))
print(html.xpath("//text()"))
```

    HEADTAIL
    ['HEAD', 'TAIL']



```python
# 甚至可以吧xpath的方法包装成函数
build_text_list = etree.XPath("//text()") # lxml.etree only!
print(build_text_list(html))
```

    ['HEAD', 'TAIL']



```python
# text也可以找爸爸
texts = build_text_list(html)
print(texts[0])

parent = texts[0].getparent()
print(parent.tag)

print(texts[1])
print(texts[1].getparent().tag)
```

    HEAD
    body
    TAIL
    br



```python
# 判断一段文本是正常的文本还是tail
print(texts[0].is_text)
print(texts[1].is_text)
print(texts[1].is_tail)
```

    True
    False
    True



```python
# 但是XPath下有些方法就不行了
stringify = etree.XPath("string()")
print(stringify(html))
print(stringify(html).getparent())
```

    HEADTAIL
    None



```python
# etree中的树是iterable的
root = etree.Element("root")
etree.SubElement(root, "child").text = "Child 1"
etree.SubElement(root, "child").text = "Child 2"
etree.SubElement(root, "another").text = "Child 3"

print(etree.tostring(root, pretty_print=True))
for element in root.iter():
    print("%s - %s" % (element.tag, element.text))
```

    b'<root>\n  <child>Child 1</child>\n  <child>Child 2</child>\n  <another>Child 3</another>\n</root>\n'
    root - None
    child - Child 1
    child - Child 2
    another - Child 3



```python
# 可以通过在iter中添加参数，来过滤输出的tag
for element in root.iter("child"):
    print("%s - %s" % (element.tag, element.text))
for element in root.iter("another", "child"):
    print("%s - %s" % (element.tag, element.text))
```

    child - Child 1
    child - Child 2
    child - Child 1
    child - Child 2
    another - Child 3



```python
# 默认情况下，所有节点都会被遍历，如Entity、Comment等，通过添加tag参数可以避免这一点
root.append(etree.Entity("#234"))
root.append(etree.Comment("some comment"))
print(etree.tostring(root))

for element in root.iter():
    if isinstance(element.tag, str):
        print("%s - %s" % (element.tag, element.text))
    else:
        print("SPECIAL: %s - %s" % (element, element.text))

for element in root.iter(tag=etree.Element):
    print("%s - %s" % (element.tag, element.text))

for element in root.iter(tag=etree.Entity):
    print(element.text)
```

    b'<root><child>Child 1</child><child>Child 2</child><another>Child 3</another>&#234;<!--some comment--></root>'
    root - None
    child - Child 1
    child - Child 2
    another - Child 3
    SPECIAL: &#234; - &#234;
    SPECIAL: <!--some comment--> - some comment
    root - None
    child - Child 1
    child - Child 2
    another - Child 3
    &#234;



```python
# 通过elementTreeName.write(filePath)可以把树写进文件或通过url传输
# 或者如果你只有elementName的话，可以这么写：
# etree.ElementTree(elementName).write(filePath)
```


```python
# 简单粗暴的直接写xml也可以
root = etree.XML(
   '<html><head/><body><p>Hello<br/>World</p></body></html>')
# 通过method参数，可以序列化为不同格式，默认method = 'xml'
print(etree.tostring(root))
print(etree.tostring(root, method='html'))
print(etree.tostring(root, method='text'))
```

    b'<html><head/><body><p>Hello<br/>World</p></body></html>'
    b'<html><head></head><body><p>Hello<br>World</p></body></html>'
    b'HelloWorld'



```python
# ElementTree class，是一个element的容器，提供了一些方法来对整个root node文档进行操作
root = etree.XML('''\
<?xml version="1.0"?>
<!DOCTYPE root SYSTEM "test" [ <!ENTITY tasty "parsnips"> ]>
<root>
  <a>&tasty;</a>
</root>
''')

tree = etree.ElementTree(root)
print(tree.docinfo.xml_version)
print(tree.docinfo.doctype)

tree.docinfo.public_id = '-//W3C//DTD XHTML 1.0 Transitional//EN'
tree.docinfo.system_url = 'file://local.dtd'
print(tree.docinfo.doctype)
```

    1.0
    <!DOCTYPE root SYSTEM "test">
    <!DOCTYPE root PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "file://local.dtd">



```python
print(etree.tostring(tree))
print(etree.tostring(tree.getroot()))
```

    b'<!DOCTYPE root PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "file://local.dtd" [\n<!ENTITY tasty "parsnips">\n]>\n<root>\n  <a>parsnips</a>\n</root>'
    b'<root>\n  <a>parsnips</a>\n</root>'



```python
# 解析文档中以string读入的xml
some_xml_data = "<root>data</root>"

# .fromstring函数其实和.XML差不多
root = etree.fromstring(some_xml_data)
print(etree.tostring(root))
```

    b'<root>data</root>'



```python
# 二进制读取解析
from io import BytesIO

some_file_or_file_like_object = BytesIO(b"<root>data</root>")
tree = etree.parse(some_file_or_file_like_object)
print(etree.tostring(tree))
# 注意：parse函数返回的是ElementTree对象，不是Element，通过getroot可以转化为Element对象
# ElementTree和Element在某些方面相似，但是方法调用上不同，比如.tag只有Element可以使用
root = tree.getroot()
print(etree.tostring(root))
try:
    print("tree执行:", tree.tag)
except:
    print("root执行:", root.tag)
```

    b'<root>data</root>'
    b'<root>data</root>'
    root执行: root



```python
# parser对象
parser = etree.XMLParser(remove_blank_text=True)
root = etree.XML("<root>  <a/>   <b>  </b>     </root>", parser)
print(etree.tostring(root))
```

    b'<root><a/><b>  </b></root>'



```python
# Incremental parsing增量解析，比如在网络分步传输的场景下需要使用
# 方法一
class DataSource:
    data = [ b"<roo", b"t><", b"a/", b"><", b"/root>" ]
    def read(self, requested_size):
        try:
            return self.data.pop(0)
        except IndexError:
            return b''

        tree = etree.parse(DataSource())
print(etree.tostring(tree))

# 方法二
parser = etree.XMLParser()

parser.feed("<roo")
parser.feed("t><")
parser.feed("a/")
parser.feed("><")
parser.feed("/root>")

root = parser.close()
print(etree.tostring(root))
```

    b'<root>data</root>'
    b'<root><a/></root>'



```python
# Event-driven parsing事件驱动的解析，如只需要很大的树中的一小部分时使用
# 方法一
# iterparse默认只解析end事件
some_file_like = BytesIO(b"<root><a>data</a></root>")
for event, element in etree.iterparse(some_file_like):
    print("%s, %4s, %s" % (event, element.tag, element.text))
    
print()

# 可以修改events参数解析所有事件
some_file_like = BytesIO(b"<root><a>data</a></root>")
for event, element in etree.iterparse(some_file_like,
                                      events=("start", "end")):
    print("%5s, %4s, %s" % (event, element.tag, element.text))
```

    end,    a, data
    end, root, None
    
    start, root, None
    start,    a, data
      end,    a, data
      end, root, None



```python
# 方法二
class ParserTarget:
    events = []
    close_count = 0
    def start(self, tag, attrib):
        self.events.append(("start", tag, attrib))
    def close(self):
        events, self.events = self.events, []
        self.close_count += 1
        return events

parser_target = ParserTarget()

parser = etree.XMLParser(target=parser_target)
events = etree.fromstring('<root test="true"/>', parser)

print(parser_target.close_count)

for event in events:
    print('event: %s - tag: %s' % (event[0], event[1]))
    for attr, value in event[2].items():
        print(' * %s = %s' % (attr, value))
```

    1
    event: start - tag: root
     * test = true


    /Users/imonce/anaconda/lib/python3.6/site-packages/ipykernel_launcher.py:15: DeprecationWarning: inspect.getargspec() is deprecated, use inspect.signature() or inspect.getfullargspec()
      from ipykernel import kernelapp as app



```python
events = etree.fromstring('<root test="true"/>', parser)
print(parser_target.close_count)
events = etree.fromstring('<root test="true"/>', parser)
print(parser_target.close_count)
```

    2
    3



```python
for event in events:
    print('event: %s - tag: %s' % (event[0], event[1]))
    for attr, value in event[2].items():
        print(' * %s = %s' % (attr, value))
```

    event: start - tag: root
     * test = true



```python
# namespace命名空间，格式为：{namespace_name}tag_name
# 一般是链接，空链接的话会 ns+编号 给一个代号
# a:b的tag名会直接报错
xhtml = etree.Element("{http://www.w3.org/1999/test}testhtml")
body = etree.SubElement(xhtml, "{http://www.w3.org/1999/test}tbody")
body.text = "Hello World"

print(etree.tostring(xhtml))

# 高贵的html
xhtml = etree.Element("{http://www.w3.org/1999/xhtml}html")
body = etree.SubElement(xhtml, "{http://www.w3.org/1999/xhtml}body")
body.text = "Hello World"

print(etree.tostring(xhtml))
```

    b'<ns0:testhtml xmlns:ns0="http://www.w3.org/1999/test"><ns0:tbody>Hello World</ns0:tbody></ns0:testhtml>'
    b'<html:html xmlns:html="http://www.w3.org/1999/xhtml"><html:body>Hello World</html:body></html:html>'



```python
# 设置默认命名空间
XHTML_NAMESPACE = "http://www.w3.org/1999/xhtml"
XHTML = "{%s}" % XHTML_NAMESPACE

NSMAP = {None : XHTML_NAMESPACE} # the default namespace (no prefix)
xhtml = etree.Element(XHTML + "html", nsmap=NSMAP) # lxml only!
body = etree.SubElement(xhtml, XHTML + "body")
body.text = "Hello World"
print(etree.tostring(xhtml))
```

    b'<html xmlns="http://www.w3.org/1999/xhtml"><body>Hello World</body></html>'



```python
# QName方法，快速合成或提取namespace
tag = etree.QName('http://www.w3.org/1999/xhtml', 'html')
print(tag.localname)
print(tag.namespace)
print(tag.text)

root = etree.Element('{http://www.w3.org/1999/xhtml}html')
tag = etree.QName(root)
print(tag.localname)
```

    html
    http://www.w3.org/1999/xhtml
    {http://www.w3.org/1999/xhtml}html
    html



```python
# nsmap方法，直接提取命名空间字典
print(xhtml.nsmap)
```

    {None: 'http://www.w3.org/1999/xhtml'}



```python
# 子元素继承父元素命名空间
root = etree.Element('root', nsmap={'a': 'http://a.b/c'})
child = etree.SubElement(root, 'child',
                         nsmap={'b': 'http://b.c/d'})
print(root.nsmap)
print(child.nsmap)
```

    {'a': 'http://a.b/c'}
    {'b': 'http://b.c/d', 'a': 'http://a.b/c'}



```python
# 添加带命名空间的属性，格式为：set({namespace_name}attr_name, attr_value)
body.set(XHTML + "bgcolor", "#CCFFAA")
print(etree.tostring(xhtml))
# 注意：get的时候也要带命名空间
print(body.get("bgcolor"))
print(body.get(XHTML+"bgcolor"))
```

    b'<html xmlns="http://www.w3.org/1999/xhtml"><body xmlns:html="http://www.w3.org/1999/xhtml" html:bgcolor="#CCFFAA">Hello World</body></html>'
    None
    #CCFFAA



```python
# 也可以用XPath
find_xhtml_body = etree.ETXPath(      # lxml only !
    "//{%s}body" % XHTML_NAMESPACE)
results = find_xhtml_body(xhtml)

print(results[0].tag)
```

    {http://www.w3.org/1999/xhtml}body



```python
# iter的话也要考虑namespace
for el in xhtml.iter('{*}body'): print(el.tag)
```

    {http://www.w3.org/1999/xhtml}body



```python
# E-factory：提供一种简单紧凑的语法来生成XML和HTML
from lxml.builder import E

def CLASS(*args): # class 是python中的保留字，无法直接当做属性名
    return {"class":' '.join(args)}

html = page = (
  E.html(       # create an Element called "html"
    E.head(
      E.title("This is a sample document")
    ),
    E.body(
      E.h1("Hello!", CLASS("title")),
      E.p("This is a paragraph with ", E.b("bold"), " text in it!"),
      E.p("This is another paragraph, with a", "\n      ",
        E.a("link", href="http://www.python.org"), "."),
      E.p("Here are some reserved characters: <spam&egg>."),
      etree.XML("<p>And finally an embedded XHTML fragment.</p>"),
    )
  )
)

print(str(etree.tostring(page, pretty_print=True),encoding='utf-8'))
```

    <html>
      <head>
        <title>This is a sample document</title>
      </head>
      <body>
        <h1 class="title">Hello!</h1>
        <p>This is a paragraph with <b>bold</b> text in it!</p>
        <p>This is another paragraph, with a
          <a href="http://www.python.org">link</a>.</p>
        <p>Here are some reserved characters: &lt;spam&amp;egg&gt;.</p>
        <p>And finally an embedded XHTML fragment.</p>
      </body>
    </html>
    



```python
# 此外还有一种基于属性的方法
from lxml.builder import ElementMaker # lxml only !

E = ElementMaker(namespace="http://my.de/fault/namespace",
                 nsmap={'p' : "http://my.de/fault/namespace"})

DOC = E.doc
TITLE = E.title
SECTION = E.section
PAR = E.par

my_doc = DOC(
  TITLE("The dog and the hog"),
  SECTION(
    TITLE("The dog", tType='title'),
    PAR("Once upon a time, ..."),
    PAR("And then ...")
  ),
  SECTION(
    TITLE("The hog"),
    PAR("Sooner or later ...")
  )
)

print(str(etree.tostring(my_doc, pretty_print=True),encoding='utf-8'))
```

    <p:doc xmlns:p="http://my.de/fault/namespace">
      <p:title>The dog and the hog</p:title>
      <p:section>
        <p:title tType="title">The dog</p:title>
        <p:par>Once upon a time, ...</p:par>
        <p:par>And then ...</p:par>
      </p:section>
      <p:section>
        <p:title>The hog</p:title>
        <p:par>Sooner or later ...</p:par>
      </p:section>
    </p:doc>
    



```python
# XPath的一些例子
root = etree.XML("<root><a x='123'>aText<b/><c/><b/></a></root>")
# 找儿子（单层子节点）
print(root.find("b"))
print(root.find("a").tag)
```

    None
    a



```python
# 找孩子（所有子节点）
print(root.find(".//b").tag)
```

    b



```python
# 带属性找孩子
print(root.findall(".//a[@x]")[0].tag)
```

    a



```python
# 通过ElementTree找路径
tree = etree.ElementTree(root)
a = root[0]
print(tree.getelementpath(a[0]))
print(tree.getelementpath(a[1]))
print(tree.getelementpath(a[2]))
```

    a/b[1]
    a/c
    a/b[2]

