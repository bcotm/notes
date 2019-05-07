# Python对象 

Python中一切都是对象！

## Python对象的4个属性：

1. id, 唯一标识一个对象。id(obj)
2. type, 对象的类型。type(obje), obj.\_\_class\_\_，再.\_\_name\_\_就是直接类名称
3. bases, 对象继承的对象。obj.\_\_bases\_\_
4. value, 对象的值。

> 对象值比较： a == b
> 对象id比较： a is b
> Python2 缓存较小范围（-1，100）的整数，所以is可能会True


## Python中只有两种对象：

1. 类：可被实例化和继承。
    1. 元类：type以及自定义元类。
    2. 类型类：object, int, list, class等。
2. 实例：不可被实例化和继承。function, module

> 如果一个对象既是类又是实例，那类的性质覆盖实例性质，即可被实例化和继承。

## Python对象之间的关系  

1. 类与类之间：issubclass(a, b), a的类型必须是类。
2. 类与实例之间：isinstance(a, b)。

> 一个对象可以既是类又是实例，比如object，int等，这些是类型类。  
> type对象可以被继承，不可以被实例化。

## Python对象关系推导规则：

1. **subclass关系可以一直传递。**
2. **instance关系成立最多一次，可延subclass关系传递。**
3. **对象都是自己的子类。**issubclass(obj, obj) == True。但__bases__中不包含，应该只是issubclass()的实现。

## Python中最基本的对象关系，type和object：  

1. **type 是 object 的子类。**
2. **object 是 type 的实例。**

> 2、3是唯一的特例，应该是在python解释器中实现的。

推导出：  
isinstance(type, type), isinstance(object, object), isinstance(type, object)都为True

## Python内部对象
1. **代码对象(可执行对象)**，是函数的一个属性，编译过的代码片段，通过compile()得到，被exec(), eval()执行。
2. **帧对象(Python执行栈帧)**，包含解释器在运行时所需所有信息。包括上一帧链接、代码对象、本地名称空间、全局名称空间、当前指令等。
3. **跟踪记录对象**。包含针对异常的栈跟踪信息。
4. **切片对象**，使用切片语法时产生。包括步进切片、多维切片、省略切片等。
5. **xrange对象**。

## Python标准对象分类

### 存储模型
1. 标量存储，保存单个字面对象。数值、字符串。
2. 容器存储，可存储不同类型对象。元组、列表、字典。

### 更新模型
1. 不可变对象。数字、字符串、元组。
2. 可变对象。列表、字典。

### 访问模型
1. 直接访问。非容器类，数字。
2. 顺序访问。序列类型，字符串、元组、列表。
3. 映射。字典。

## Python内建函数
1. type(), isinstance()
2. cmp()
3. str(), repr()。obj = eval(repr(obj))

## Python中各种对象所属范围


特例：  
```python
class c:
    pass
isinstance(c, type)
# Py2 False, Py3 True
# 猜测应该是Py3默认class都继承object
```

## 附
### 1. 推导type和type，type和object，object和object的实例关系

```python
# type 和 type, instance关系通过object传递
issubclass(type, object)
isinstance(object, type)
# object 和 object
isinstance(object, type)
issubclass(type, object)
# type 和 object
issubclass(type, object)
isinstance(object, object)
```

### 2. 获取任何对象的直系类型

```python
a = 1.3
a.__class__.__name__
```

### 3. object是所有类型的超类和类型？type呢？

http://www.cs.utexas.edu/~cannata/cs345/Class%20Notes/15%20Python%20Types%20and%20Objects.pdf