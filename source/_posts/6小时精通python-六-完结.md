---
title: 6小时精通python(六-完结)
date: 2019-04-19 17:35:34
tags: [6小时精通python, python, python3, python精通]
---

译自：[https://learnxinyminutes.com/docs/python3/](https://learnxinyminutes.com/docs/python3/)

```python
####################################################
## 6. Classes
####################################################
```


```python
# 通过class声明，来创建一个类
# 类内方法中，self为一个保留字，代表类实例化后instance自身
# 类内方法中，cls也是一个保留字，代表类class自身
# 通过self.***可以给类内属性赋值，或调用类内方法
class Human:

    # 直接定义的变量，是这个类的共享属性，所有实例都可以访问
    species = "H. sapiens"

    # __init__是一个保留方法，用于类的实例化（生成实例时自动调用）
    # 注意：名称前后有双下划线__，代表这个对象或者属性是python调用、用户定义的
    # 这类方法（对象、属性）包括: __init__, __str__, __repr__ etc.
    # 这类特殊方法，也被称作（dunder method）
    # 不要自己创造这类方法
    def __init__(self, name):
        # 将参数分配给实例的name属性
        self.name = name

        # 初始化属性
        self._age = 0

    # 这是类的一个内建方法，所有内建的方法都需要把self作为其第一个形式参数
    def say(self, msg):
        print("{name}: {message}".format(name=self.name, message=msg))

    # 另一个方法
    def sing(self):
        return 'yo... yo... microphone check... one two... one two...'

    # @classmethod是一个声明，声明接下来定义的方法是该类所有实例的共享方法
    # 这种方法被调用时，必须有cls作为第一个参数
    # 类方法的特点在于，可以被类自身调用，如Human.get_species()
    @classmethod
    def get_species(cls):
        return cls.species

    # @staticmethod声明接下来定义的是一个静态方法
    # 静态方法可以被类单独调用
    @staticmethod
    def grunt():
        return "*grunt*"

    # @property就是一个getter，声明该方法用于访问内部属性
    # @property这个声明，将age()方法转换为同名的只读属性。 
    # 但是，不需要在Python中编写琐碎的getter和setter。
    @property
    def age(self):
        return self._age

    # 如果还想要让该属性可更改，可以这么写
    @age.setter
    def age(self, age):
        self._age = age

    # deleter可以让该属性可删除
    @age.deleter
    def age(self):
        del self._age
```


```python
# __name__代表的是运行进程的名称
# __name__ == '__main__'，判断用户是否是将该python文件当作主要脚本运行
# 简单来说，if __name__ == '__main__':代码块中的内容
# 只有在运行该python文件时才会生效，如果该python文件是以import形式被调用，则不会运行
# 而写在if __name__ == '__main__':代码块外的内容，被import时，也会运行
if __name__ == '__main__':
    # 生成Human类的实例
    # 类名加括号，直接调用__init__方法
    i = Human(name="Ian")
    i.say("hi")                     # "Ian: hi"
    j = Human("Joel")
    j.say("hello")                  # "Joel: hello"
    # i and j 是Human类的两个实例
    # 调用类方法
    i.say(Human.get_species())          # "Ian: H. sapiens"
    # 共享属性改了之后，大家都改了
    Human.species = "H. neanderthalensis"
    i.say(i.get_species())          # => "Ian: H. neanderthalensis"
    j.say(j.get_species())          # => "Joel: H. neanderthalensis"

    # 类可以调用静态函数
    print(Human.grunt())            # => "*grunt*"
    
    # 有些版本中实例是不能调用静态函数的
    print(i.grunt())
                                    
    # 更新实例的属性
    i.age = 42
    # 获取property
    i.say(i.age)                    # => "Ian: 42"
    j.say(j.age)                    # => "Joel: 0"
    # 删除i的age属性
    del i.age
```

    Ian: hi
    Joel: hello
    Ian: H. sapiens
    Ian: H. neanderthalensis
    Joel: H. neanderthalensis
    *grunt*
    *grunt*
    Ian: 42
    Joel: 0



```python
# 再访问i的年龄就会报错
i.age
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-207-993258cc61d3> in <module>()
          1 # 再访问i的年龄就会报错
    ----> 2 i.age
    

    <ipython-input-186-b3205f030117> in age(self)
         44     @property
         45     def age(self):
    ---> 46         return self._age
         47 
         48     # 如果还想要让该属性可更改，可以这么写


    AttributeError: 'Human' object has no attribute '_age'



```python
# 不仅仅是age()没了，_age这个属性是真的没了
i._age
```


    ---------------------------------------------------------------------------

    AttributeError                            Traceback (most recent call last)

    <ipython-input-208-4ea879b64970> in <module>()
          1 # 不仅仅是age()没了，_age这个属性是真的没了
    ----> 2 i._age
    

    AttributeError: 'Human' object has no attribute '_age'



```python
####################################################
## 6.1 继承
####################################################
```


```python
# 继承允许定义新的子类，这些子类从父类继承方法和变量
```


```python
# 使用上面定义的Human类作为基类或父类，我们可以定义一个子类Superhero
# 它继承了类的变量如“species”，“name”和“age”，
# 以及“sing”和“grunt”等方法
# 但superhero也可以拥有自己的属性

# 如果要将文件模块化，您可以将上面的类放在自己的文件中，命名为human.py

# 要从其他文件导入功能，请使用以下格式
# from “filename（不加扩展名.py）” import “函数名或类名”
from human import Human
```


```python
# 将父类当作参数写进子类定义作为声明
# 如：class child(parent):

class Superhero(Human):

    # 如果您想让子类继承父类的所有定义且没有任何修改
    # 您可以只使用“pass”关键字（而不使用其他关键字）
    # 如
    #     class Human2(Human):
    #         pass
    
    # 子类可以重写其父类的属性
    species = 'Superhuman'

    # 子类自动继承其父类的构造函数（__init__），包括它的参数
    # 但也可以定义其他参数或定义并重写其方法
    # 此构造函数从“human”类继承“name”参数
    # 并且添加“superpower”和“movie”参数：
    def __init__(self, name, movie=False,
                 superpowers=["super strength", "bulletproofing"]):

        # 增加新的属性
        self.fictional = True
        self.movie = movie
        # 注意可变的默认值，因为默认值是共享的
        self.superpowers = superpowers

        # “super”是一个保留函数，该函数允许您访问父类的方法
        # 下面的语句将调用父类构造函数：
        super().__init__(name)

    # 覆盖sing方法
    def sing(self):
        return 'Dun, dun, DUN!'

    # 增加实例方法
    def boast(self):
        for power in self.superpowers:
            print("I wield the power of {pwr}!".format(pwr=power))
```


```python
if __name__ == '__main__':
    sup = Superhero(name="Tick")

    # 通过isinstance方法，可以判断，实例和类的关系
    if isinstance(sup, Human):
        print('I am human')
    # 通过type(instance)可以得到实例的class对象
    if type(sup) is Superhero:
        print('I am a superhero')

    # 通过__mro__方法，可以获取类的继承链（super方法或者getattr方法）
    print(Superhero.__mro__)    # => (<class '__main__.Superhero'>, <class '__main__.Human'>, <class 'object'>)

    # 使用父类方法，访问子类属性
    print(sup.get_species())    # => Superhuman

    # 调用覆盖了的方法
    print(sup.sing())           # => Dun, dun, DUN!

    # 调用父类的方法
    sup.say('Spoon')            # => Tick: Spoon

    # 调用子类独有的方法
    sup.boast()                 # => I wield the power of super strength!
                                # => I wield the power of bulletproofing!

    # 继承了的类属性
    sup.age = 31
    print(sup.age)              # => 31

    # 子类独有的属性
    print('Am I Oscar eligible? ' + str(sup.movie))
```

    I am human
    I am a superhero
    (<class '__main__.Superhero'>, <class '__main__.Human'>, <class 'object'>)
    Superhuman
    Dun, dun, DUN!
    Tick: Spoon
    I wield the power of super strength!
    I wield the power of bulletproofing!
    31
    Am I Oscar eligible? False



```python
####################################################
## 6.2 多重继承
####################################################
```


```python
# 定义一个蝙蝠类
class Bat:

    species = 'Baty'

    def __init__(self, can_fly=True):
        self.fly = can_fly

    # 这个类页游say的方法
    def say(self, msg):
        msg = '... ... ...'
        return msg

    # 还有独有的方法
    def sonar(self):
        return '))) ... ((('
```


```python
if __name__ == '__main__':
    b = Bat()
    print(b.say('hello'))
    print(b.fly)
```

    ... ... ...
    True



```python
# 如果您写了多个文件，就需要导入一下
from superhero import Superhero
from bat import Bat
```


```python
# 定义蝙蝠侠，继承自超级英雄和蝙蝠两个类
class Batman(Superhero, Bat):

    def __init__(self, *args, **kwargs):
        # 通常，要继承属性，必须调用super
        # 然而，我们在这里处理多个继承
        # 而super（）只适用于MRO列表中的下一个基类。
        # 因此，我们明确地为所有祖先(父类)调用__init__
        # 使用“*args”和“*kwargs”可以以一种干净的方式传递参数
        # 每个父类“剥一层洋葱皮”
        Superhero.__init__(self, 'anonymous', movie=True, 
                           superpowers=['Wealthy'], *args, **kwargs)
        Bat.__init__(self, *args, can_fly=False, **kwargs)
        # override the value for the name attribute
        self.name = 'Sad Affleck'

    def sing(self):
        return 'nan nan nan nan nan batman!'
```


```python
if __name__ == '__main__':
    sup = Batman()

    
    # 通过__mro__方法，可以获取类的继承链（super方法或者getattr方法）
    print(Batman.__mro__)       # => (<class '__main__.Batman'>, 
                                # => <class 'superhero.Superhero'>, 
                                # => <class 'human.Human'>, 
                                # => <class 'bat.Bat'>, <class 'object'>)

    # 调用父类方法获取子类属性
    print(sup.get_species())    # => Superhuman

    # 调用覆盖后的方法
    print(sup.sing())           # => nan nan nan nan nan batman!

    # 两个父类有重名方法时，顺序在前的优先级更高
    sup.say('I agree')          # => Sad Affleck: I agree

    # 调用第二父类方法
    print(sup.sonar())          # => ))) ... (((

    # 继承类属性
    sup.age = 100
    print(sup.age)              # => 100

    # 输出从第二父类继承的属性，该属性已被覆盖
    print('Can I fly? ' + str(sup.fly)) # => Can I fly? False

```

    (<class '__main__.Batman'>, <class '__main__.Superhero'>, <class '__main__.Human'>, <class '__main__.Bat'>, <class 'object'>)
    Superhuman
    nan nan nan nan nan batman!
    Sad Affleck: I agree
    ))) ... (((
    100
    Can I fly? False



```python
####################################################
## 7. Advanced
####################################################
```


```python
# 生成器可以帮你偷很多懒
def double_numbers(iterable):
    for i in iterable:
        yield i + i
```


```python
# 生成器可以节省很多内存
# 因为它们只加载所需处理iterable中的下一个值的数据(边生成边处理)
# 普通方法需要 先生成后处理
# 这使其可以进行大范围的数据操作（其他方法可能不行）
# 注意：python 3中，“range”替换了“xrange”
for i in double_numbers(range(1, 900000000)):  # `range` is a generator.
    print(i)
    if i >= 30:
        break
```

    2
    4
    6
    8
    10
    12
    14
    16
    18
    20
    22
    24
    26
    28
    30



```python
# 正如可以创建列表理解一样，也可以创建生成器理解
# 这里，圆括号是关键，你以为是tuples，实际上是生成器
values = (-x for x in [1,2,3,4,5])
print(values)
for x in values:
    print(x)  # prints -1 -2 -3 -4 -5 to console/terminal
```

    <generator object <genexpr> at 0x102e9c990>
    -1
    -2
    -3
    -4
    -5



```python
# 也可以直接把一个生成器理解投射到list上
values = (-x for x in [1,2,3,4,5])
gen_to_list = list(values)
print(gen_to_list)  # => [-1, -2, -3, -4, -5]
```

    [-1, -2, -3, -4, -5]



```python
# 修饰器
from functools import wraps


def beg(target_function):
    
    @wraps(target_function)
    def wrapper(*args, **kwargs):
        msg, say_please = target_function(*args, **kwargs)
        if say_please:
            return "{} {}".format(msg, "Please! I am poor :(")
        return msg

    return wrapper

# 这里通过beg修饰say
# 可以改变say的输出
@beg
def say(say_please=False):
    msg = "Can you buy me a beer?"
    return msg, say_please
```


```python
print(say())                 # Can you buy me a beer?
print(say(say_please=True))  # Can you buy me a beer? Please! I am poor :(
```

    Can you buy me a beer?
    Can you buy me a beer? Please! I am poor :(

