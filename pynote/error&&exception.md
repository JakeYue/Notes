# python3的错误和异常

#### python中有2种容易识别的错误:

- 语法错误: 语法错误或者称之为解析错.

```python
In [4]: for i in l
  File "<ipython-input-4-a28d2c74d4d7>", line 1
    for i in l
              ^
SyntaxError: invalid syntax
“^” 这个符号提示错误的开始位置
```

- 异常 : 即便 Python 程序的语法是正确的，在运行它的时候，也有可能发生错误。运行期检测到的错误被称为异常.

    ```python
    In [5]: 10 * (1/0) '''0不能作为除数'''
    ----------------------------------------------------------------
    ZeroDivisionError              Traceback (most recent call last)
    <ipython-input-5-0b280f36835c> in <module>
    ----> 1 10 * (1/0)
    
    ZeroDivisionError: division by zero
    
    In [7]: print(name)'''name没有被定义'''
    ----------------------------------------------------------------
    NameError                      Traceback (most recent call last)
    <ipython-input-7-9ba126b17b03> in <module>
    ----> 1 print(name)
    
    NameError: name 'name' is not defined
        
    In [8]: '2' + 2'''不能与str相加'''
    ----------------------------------------------------------------
    TypeError                      Traceback (most recent call last)
    <ipython-input-8-d2b23a1db757> in <module>
    ----> 1 '2' + 2
    
    TypeError: can only concatenate str (not "int") to str
        
    ''' 异常以不同的类型出现，这些类型都作为信息的一部分打印出来: 例子中的类型有 ZeroDivisionError，NameError 和 TypeError.
    	错误信息的前面部分显示了异常发生的上下文，并以调用栈的形式显示具体信息 '''
    
    ```

#### 异常处理

- 异常捕捉可以使用`try/except`  语句.

![img](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/try_except.png2021%2009%2023%2015%2042%2005)

**try 语句按照如下方式工作**

- 首先，执行 try 子句（在关键字 try 和关键字 except 之间的语句）。
- 如果没有异常发生，忽略 except 子句，try 子句执行后结束。
- 如果在执行 try 子句的过程中发生了异常，那么 try 子句余下的部分将被忽略。如果异常的类型和 except 之后的名称相符，那么对应的 except 子句将被执行。
- 如果一个异常没有与任何的 except 匹配，那么这个异常将会传递给上层的 try 中。

---

`try/except...else`语句:

`try/except`语句还有一个可选的 **else** 子句，如果使用这个子句，那么必须放在所有的 except 子句之后。else 子句将在 try 子句没有发生任何异常的时候执行.

![img](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/try_except_else.png2021%2009%2023%2015%2048%2027)

`try-finally` 语句:

`try-finally` 语句无论是否发生异常都将执行最后的代码.

![img](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/try_except_else_finally.png2021%2009%2023%2015%2049%2056)

#### 抛出异常

Python 使用 raise 语句抛出一个指定的异常。

raise语法格式:`raise [Exception [, args [, traceback]]]`

![img](https://raw.githubusercontent.com/JakeYue/picgo/master/uPic/raise.png2021%2009%2023%2015%2050%2051)

- 异常链

    - raise 语句支持可选的from子句,该句子用于启用链式异常.

        ```python
        >>> def func():
        ...     raise IOError
        ...
        >>> try:
        ...     func()
        ... except IOError as exc:
        ...     raise RuntimeError('Failed to open database') from exc
        ...
        Traceback (most recent call last):
          File "<stdin>", line 2, in <module>
          File "<stdin>", line 2, in func
        OSError
        
        The above exception was the direct cause of the following exception:
        
        Traceback (most recent call last):
          File "<stdin>", line 4, in <module>
        RuntimeError: Failed to open database异常链在 [`except`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#except) 或 [`finally`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#finally) 子句触发异常时自动生成。禁用异常链可使用 `from None` 习语：
        ```

- 异常链在 [`except`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#except) 或 [`finally`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#finally) 子句触发异常时自动生成。禁用异常链可使用 `from None` 习语：

```shell
>>> try:
...     open('database.sqlite')
... except OSError:
...     raise RuntimeError from None
...
Traceback (most recent call last):
  File "<stdin>", line 4, in <module>
RuntimeError
```

#### 用户自定义异常(来自runoob.com)

你可以通过创建一个新的异常类来拥有自己的异常。异常类继承自 Exception 类，可以直接继承，或者间接继承，例如:

```python 
>>> class MyError(Exception):
        def __init__(self, value):
            self.value = value
        def __str__(self):
            return repr(self.value)
   
>>> try:
        raise MyError(2*2)
    except MyError as e:
        print('My exception occurred, value:', e.value)
   
My exception occurred, value: 4
>>> raise MyError('oops!')
Traceback (most recent call last):
  File "<stdin>", line 1, in ?
__main__.MyError: 'oops!'
```

在这个例子中，类 Exception 默认的 __init__() 被覆盖。当创建一个模块有可能抛出多种不同的异常时，一种通常的做法是为这个包建立一个基础异常类，然后基于这个基础类为不同的错误情况创建不同的子类:

```python
class Error(Exception):
    """Base class for exceptions in this module."""
    pass

class InputError(Error):
    """Exception raised for errors in the input.

    Attributes:
        expression -- input expression in which the error occurred
        message -- explanation of the error
    """

    def __init__(self, expression, message):
        self.expression = expression
        self.message = message

class TransitionError(Error):
    """Raised when an operation attempts a state transition that's not
    allowed.

    Attributes:
        previous -- state at beginning of transition
        next -- attempted new state
        message -- explanation of why the specific transition is not allowed
    """

    def __init__(self, previous, next, message):
        self.previous = previous
        self.next = next
        self.message = message
        
'''大多数的异常的名字都以"Error"结尾，就跟标准的异常命名一样'''
```

