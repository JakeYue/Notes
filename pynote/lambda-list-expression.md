## Lambda 表达式

- `lambda` 关键字用于创建小巧的匿名函数.

- lambda函数的语法只包含一个语句

- 还可以把匿名函数用作传递的实参

    `lambda [arg, [argn, ....]]:expression`

    > lambda只是一个表达式，函数体比def简单很多。
    >
    > lambda的主体是一个表达式，而不是一个代码块。仅仅能在lambda表达式中封装有限的逻辑进去。
    >
    > lambda函数拥有自己的命名空间，且不能访问自有参数列表之外或全局命名空间里的参数。
    >
    > 虽然lambda函数看起来只能写一行，却不等同于C或C++的内联函数，后者的目的是调用小函数时不占用栈内存从而增加运行效率

```python
def make_incrementor(n):
	return lambda x: x + n

f = make_incrementor(42)

# 当作实参传入
pairs = [(1, 'one'), (2, 'two'), (3, 'three'), (4, 'four')]
pairs.sort(key=lambda pair: pair[1])
pairs
[(4, 'four'), (1, 'one'), (3, 'three'), (2, 'two')]
```

---

### 列表推导式

- 常见的用法为:对序列或可迭代对象中的每个元素应用某种操作，用生成的结果创建新的列表；或用满足特定条件的元素创建子序列.

> 列表推导式可以利用 range 区间、元组、列表、字典和集合等数据类型，快速生成一个满足指定需求的列表

列表推导式语法:

`[表达式 for 迭代变量 in 可迭代对象 [if 条件表达式]]` ( if条件是可选的)

```python
a_range = range(10)
# 对a_range执行for表达式
a_list = [x * x for x in a_range]
# a_list集合包含10个元素
print(a_list)

# 多for表达
d_list = [(x, y) for x in range(5) for y in range(4)]
e_list = [[x, y, z] for x in range(5) for y in range(4) for z in range(6)]

# 带条件表达
src_a = [30, 12, 66, 34, 39, 78, 36, 57, 121]
src_b = [3, 5, 7, 11]
# 只要y能整除x，就将它们配对在一起
result = [(x, y) for x in src_b for y in src_a if y % x == 0]
print(result)

# if...else
a=[i if i%2==0 else 'qi' for i in range(10)]
print(a)
```
