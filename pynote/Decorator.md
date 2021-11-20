# python装饰器

==**Python装饰器本质上是对函数闭包的语法糖.**==

- 闭包函数: 函数闭包本质上是一个函数,它的接收参数和返回值也都是函数,返回的函数本质上是对传入的参数进行增强之后的结果.
- 语法糖: 语法糖指计算机语言中添加的某种语法，这种语法对语言的功能没有影响，但是更方便程序员使用.
    - 语法糖**没有增加新功能**(解释器本身就是闭包封装而已),只是一种更方便的写法.
    - 语法糖可以**完全等价地转换**为原本非语法糖的代码
- 装饰器: 本质上是对函数闭包的语法糖,就是闭包函数的使用.
    - 装饰器的增强时机: 在第一次调用之前,只有在第一次调用被装饰的函数时,闭包函数才会被调用.若整个程序的运行过程中都没有调用被装饰的函数,闭包函数也不会被调用
    - 装饰器增强的次数: 只增强一次.也就是说,闭包函数只被调用一次,当第二次以上调用原函数时,实际上调用的直接就是增强后的函数.

函数:

```python
def doce_1():
   ...:     print("主函数")
```



闭包函数:

```python
def en_doce(func):
   ...:     def doce_2():
   ...:         func()
   ...:         print("en")
   ...:     return doce_2

实例化闭包函数:
    doc = en_doce(doce_1)
    
调用闭包函数:
In [8]: doc()
主函数
en
```

语法糖:

```python
def en_doce(func):
   ...:     def doce_2():
   ...:         func()
   ...:         print("en")
   ...:     return doce_2

@ 加上闭包函数名就是语法糖.
@en_doce == doc = en_doce(doce_1),等价于;
In [10]: @en_doce
    ...: def doce_1():
    ...:     print("主要函数")

        
        
In [11]: doce_1()
主要函数
en


In [12]: print(doce_1.__name__)
doce_2
```

---

> 函数作用域:

- 作用域,是栋楼,下楼套上楼;
- 读变量,往下搜,一直到一楼;
- 改变量,莫下楼,除非你放狗(global).

> 闭包和装饰器:

- 闭包: 至少2层楼,下楼变量管上楼,return上楼不动手;
- 装饰器: 客人空手来,还得请上楼,干啥都同意,有参给上楼.

-----

#### 可以传参的装饰器

---

```python
def en_doce(func):
   ...:     def doce_2(*args, **kwargs):
   ...:         func(*args, **kwargs)
   ...:         print("en")
   ...:     return doce_2
```
