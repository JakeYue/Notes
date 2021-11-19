### json 语法规则

---

- 数据在名称/值对中: `"key":"value"`

- 数据用逗号分隔:`"key":"value","key":"value"`

- 花括号保存对象:`{"key":"value","key":"value}`

- 方括号保存数组:`[1,2,3]`

- ```json
    {
      "name":"jake",
      "object":{"name":"jake","age":12,"lastname":"123"},
      "array":["name":"jake","age":12,"lastname":"123",{"name":"jake","age":12,"lastname":"123"}],
    true,
    false,
    null
    }
    ```

### json 数据类型

---

> 1. 数字(整数、浮点数)
> 2. 字符串(在双引号中)
> 3. 逻辑值(true或false)
> 4. 数组(方扣号中)
> 5. 对象(在花扣号中)
> 6. null

- json对象可以有多个名称/值对,json数组可以包含有多个对象.

---

### python解析json的流程

**使用时需要import json导入**

- json.dumps()：将python对象编码为json的字符串
- json.loads():将字符串编码为一个python对象
- json.dump():将python对象序列化到一个文件，是文本文件，相当于将序列化后的json字符写到一个文件
- json.load():从文件反序列表出python对象

> 总结：不带s的是序列化到文件或者从文件反序列化，带s的都是内存操作不涉及持久化

### python编码为json类型转换对应表:

| Python                                 | JSON   |
| :------------------------------------- | ------ |
| dict                                   | object |
| list, tuple                            | array  |
| str                                    | string |
| int, float, int- & float-derived Enums | number |
| True                                   | true   |
| False                                  | false  |
| None                                   | null   |

```python
In [1]: import json
In [2]: a = [{1:12, "a":12.3}, [1,2,3], (1,2), "asd", "ad", 12,12,3.3, True, False, None]
In [3]: print("python:\n",a)
python:
 [{1: 12, 'a': 12.3}, [1, 2, 3], (1, 2), 'asd', 'ad', 12, 12, 3.3, True, False, None]

In [4]: print("json:\n", json.dumps(a))
json:
 [{"1": 12, "a": 12.3}, [1, 2, 3], [1, 2], "asd", "ad", 12, 12, 3.3, true, false, null]

In [5]:
```

### json.dumps():

- 将一个Python数据类型列表编码成json格式的字符串:`json.dumps([1,2,3]) =>'[1,2,3]'`

 json.dumps()函数常用参数的使用:

​	函数原型:`dumps(obj, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, sort_keys=False, **kw)`该方法返回编码后的一个json字符串，是一个str对象encodedjson.

- sort_keys: 是否按字典排序（a到z）输出，默认编码成json格式字符串后，是紧凑输出，并且也没有顺序的，不利于可读。sort_keys等于True表示升序.

```python
In [6]: data = [{"a":"A", "x":(2,4), 'c':3.0}]

In [7]: print(json.dumps(data))
[{"a": "A", "x": [2, 4], "c": 3.0}]

In [8]: print(json.dumps(data, sort_keys=True))
[{"a": "A", "c": 3.0, "x": [2, 4]}]
```

- indent: 根据数据格式缩进显示，读起来更清晰，indent的数值表示缩进的位数

- ```python
    print(json.dumps(data, sort_keys=True, indent=2))
    [
      {
        "a": "A",
        "c": 3.0,
        "x": [
          2,
          4
        ]
      }
    ]
    ```

- separators: 去掉逗号“,”和冒号“:”后面的空格; 该参数是元组格式的。

- ```python
    print(json.dumps(data, separators=(',', ':')))
    [{"a":"A","x":[2,4],"c":3.0}]
    ```

- shipkeys: 在encoding的过程中，dict的对象只能是基本数据类似（int，float,bool,None,str）,如果是其它类型，那么在编码的过程中就会抛出ValueError异常。shipkeys可以跳过那些非string对象的key的处理，就是不处理.

- ensure_ascii: 表示编码使用的字符集，默认是True,表示使用ascii码进行编码。如果设置为False，就会以Unicode进行编码

- default: 将类对象编码成Json串,Python中的dict对象可以直接序列化为json的{}.用class表示对象时,类不是一个可以直接序列化的对象,但我们可以使用dumps()函数中的default参数来实现，由两种方法:

    1. 在类内部定义一个obj_json 方法将对象的属性转换成dict，然后再被序列化为json。
    2. 通过__dict__属性，通常class及其实例都会有一个__dict__属性（除非类中添加了__slots__属性），它是一个dict类型，存储的是类或类实例中有效的属性

---

### json.loads():将json的字符串解码成python对象

```python
In [12]: json.loads('{"a":"b"}')
Out[12]: {'a': 'b'}
```

> Python中的list和tuple都被转化成json的数组，而解码后，json的数组最终被转化成Python的list的，无论是原来是list还是tuple.

#### JSON 解码为 Python 类型转换对应表:

| JSON          | Python |
| :------------ | :----- |
| object        | dict   |
| array         | list   |
| string        | str    |
| number (int)  | int    |
| number (real) | float  |
| true          | True   |
| false         | False  |
| null          | None   |

```python
a = [{"1": 12, "a": 12.3}, [1, 2, 3], (1, 2), "asd", "ad", 12, 12, 3.3, True, False, None]

print("python:\n", a)
python:#编码前
 [{'1': 12, 'a': 12.3}, [1, 2, 3], (1, 2), 'asd', 'ad', 12, 12, 3.3, True, False, None]

print("json:\n", json.dumps(a))
json:#编码后
 [{"1": 12, "a": 12.3}, [1, 2, 3], [1, 2], "asd", "ad", 12, 12, 3.3, true, false, null]

 print("json:\n", json.loads(json.dumps(a)))
python:#解码后
 [{'1': 12, 'a': 12.3}, [1, 2, 3], [1, 2], 'asd', 'ad', 12, 12, 3.3, True, False, None]

```

> json格式的字符串解码成Python对象以后，String类型都变成了Unicode类型，数组变成了list，不会回到原来的元组类型，字典key的字符类型也被转成Unicode类型.

**json反序列化为类对象或者类的实例，使用的是loads()方法中的object_hook参数**

