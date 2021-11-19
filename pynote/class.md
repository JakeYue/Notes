# 类的定义和实例创建

#### 定义类

```python
# 通过class关键字定义,类名通常习惯首字母大写,python中的类都继承于object类
## 语法
class Work(object):
     '''下面可以添加属性和方法'''
    pass
	pass
# 注意：我们定义的类都会继承于object类，当然也可以不继承object类；两者区别不大，但没有继承于object类使用多继承时可能会出现问题
```

#### 创建实例

```python
example1 = Work()
example2 = Work()
# 实例创建完毕后,就可以使用Work类的属性和方法了
```

#### 类的属性

- 类属性可以分为:
    - 实例属性:用于区分不同的实例
    - 类属性:每个实例的共有属性

- 实例属性每个实例都各自拥有，相互独立；而类属性有且只有一份，是共有的属性

```python
# 类变量
class Work(object):
    '''类变量,name'''
    name = "1" 
    pass
# 实例变量
class Work(object):
    '''实例变量,name'''
    def names(self):
        self.namenum = name
        pass
# 类方法(实例方法)
class Work(object):
    '''类方法,names'''
    def names(self):
        self.namenum = name
        pass
if __name__ == '__main__':
    
#  访问类变量
print(Work.name)

# 访问类方法,需要先实例化类
p = Work() # 实例化类
p.names() # 访问类方法

# 可以实例化多个类
p = Work()
p1 = Work()
p2 = Work()

# 类变量的修改
'''使用实例修改类变量,只是修改实例化后的变量(没有修改类变量,只是指向了新的变量'''
p.name = "5" # 其他实例中的name 还是"1",p1.name = "1"
'''使用类名. 来修改类变量,将影响所有实例'''
Work.name = "5" # 所有实例都将受到影响,p.name = "5",p1.name = "5"
```

#### 使用构造器创建实例变量

```python
# 语法
class Work(object):
    '''下行为构造器'''
    def __init__(self,name):
        self.names = name
        
        '''方法'''
    def say(self,name):
        pass
 '''self 默认关键字,代表实例对象.
 类的方法与普通的函数只有一个特别的区别——它们必须有一个额外的第一个参数名称, 按照惯例它的名称是 self'''
```

#### 类的继承

- 面向对象的编程带来的主要好处之一是代码的重用，实现这种重用的方法之一是通过继承机制。通过继承创建的新类称为**子类**或**派生类**，被继承的类称为**基类**、**父类**或**超类**

```python
# 继承语法
class 派生类名(基类名)
    ...
 '''python中继承中的一些特点:
1、如果在子类中需要父类的构造方法就需要显式的调用父类的构造方法，或者不重写父类的构造方法。
2、在调用基类的方法时，需要加上基类的类名前缀，且需要带上 self 参数变量。区别在于类中调用普通函数时并不需要带上 self 参数
3、Python 总是首先查找对应类型的方法，如果它不能在派生类中找到对应的方法，它才开始到基类中逐个查找。（先在本类中查找调用的方法，找不到才去基类中找）。'''
'''
如果在继承元组中列了一个以上的类，那么它就被称作"多重继承"
# 语法：
派生类的声明，与他们的父类类似，继承的基类列表跟在类名之后，如下所示：
class SubClassName (ParentClass1[, ParentClass2, ...]):
    ...
'''
```



#### 类的私有属性

- **__private_attrs**：两个下划线开头，声明该属性为私有，不能在类的外部被使用或直接访问。在类内部的方法中使用时 **self.__private_attrs**。

#### 类的私有方法

- **__private_method**：两个下划线开头，声明该方法为私有方法，不能在类的外部调用。在类的内部调用 **self.__private_methods**

```python
class JustCounter:
    __secretCount = 0  # 私有变量
    publicCount = 0    # 公开变量
 
    def count(self):
        self.__secretCount += 1
        self.publicCount += 1
        print self.__secretCount
 
counter = JustCounter()
counter.count()
counter.count()
print counter.publicCount
print counter.__secretCount  # 报错，实例不能访问私有变量

访问可以再内部设置或者获取私有变量值
@property 获取变量值
@xxx.settet 设置变量值 (需要跟上面配合使用)
self.__name = name
@property
def name(self):
    return self.__name
@name.settet
def name(self,value):
    self.__name = value
```

- Python不允许实例化的类访问私有数据，但你可以使用 **object._className__attrName**（ **对象名._类名__私有属性名** ）访问属性

#### 方法重写

如果你的父类方法的功能不能满足你的需求，你可以在子类重写你父类的方法.

```python
class Parent:        # 定义父类
   def myMethod(self):
      print '调用父类方法'
 
class Child(Parent): # 定义子类
   def myMethod(self):
      print '调用子类方法'
 
c = Child()          # 子类实例
c.myMethod()         # 子类调用重写方法

#单继承,如果要使用重写前的方法(父类方法),可以使用
super().myMethod(self)
# 多继承中supre调用的是根据RMO列表中当前实例中类在RMO列表的下一个类
class Base(object):
    def __init__(self):
        print("enter Base")
        print("leave Base")
 
class A(Base):
    def __init__(self):
        print("enter A")
        super(A, self).__init__()
        print("leave A")
 
class B(Base):
    def __init__(self):
        print("enter B")
        super(B, self).__init__()
        print("leave B")
 
class C(A, B):
    def __init__(self):
        print("enter C")
        super(C, self).__init__()
        print("leave C")
c = C()
# RMO:C->A->B-object
'''
super实现原理：通过c3算法，生成mro（method resolution order）列表，根据列表中元素顺序查询调用.
新式类调用顺序为广度优先，旧式类为深度优先.
“查找子类的上层父类的下一个类”
三个原则: 子类在父类前，所有类不重复调用，从左到右
1、子类永远在父类前面
2、如果有多个父类，会根据它们在列表中的顺序被检查
3、如果对下一个类存在两个合法的选择，选择第一个父类
'''
```



#### 单下划线、双下划线、头尾双下划线说明：

- **__foo__**: 定义的是特殊方法，一般是系统定义名字 ，类似 **__init__()** 之类的。
- **_foo**: 以单下划线开头的表示的是 protected 类型的变量，即保护类型只能允许其本身与子类进行访问，不能用于 **from module import \***
- **__foo**: 双下划线的表示的是私有类型(private)的变量, 只能是允许这个类本身进行访问了。

#### 类多态

- 使用同一种方法,传入不同的实例,等到不同的结果

参数传递图示:

![截屏2021-09-22 下午6.25.06](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/%E6%88%AA%E5%B1%8F2021-09-22%20%E4%B8%8B%E5%8D%886.25.06.png2021%2009%2023%2008%2052%2038)



类思维导图:

![v2-21b16776e5a46e666a6af75a973c4ae5_r](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/v2-21b16776e5a46e666a6af75a973c4ae5_r.jpeg2021%2009%2023%2008%2052%2048)