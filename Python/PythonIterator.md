# Python 迭代器、生成器、装饰器、描述符

## 迭代器

1. 容器:包含元素的数据结构。
2. 迭代(Iteration)：可从某事物中一个一个取出。
3. 可迭代对象(Iterable)：可以用在loop中，可以返回一个迭代器，实现了\_\_iter\_\_方法。
4. 迭代器(Iterator)：为了返回Iterable对象中下一个元素，内部保存了位置状态，比如当前返回的最后一个index，实现了\_\_next\_\_函数，可raise StopIteration。是一个helper object。
5. 生成器(Generator):简洁的方法生成迭代器，只能被使用一次。通过yield函数返回。
> iterator = iter(iterable)
> itertools包中函数都返回一个迭代器。

```python
# 类类型的迭代器
class IteraX: #这里如果继承object为何会有问题
    # internal state
    def __init__():
        self.prev = 0
        self.curr = 1
    # Iterable
    def __iter__(self):
        return self
    # Iterator
    def __next__(self):
        value = self.curr
        self.curr += self.prev
        self.prev = value
        return value
```
# 函数生成器

生成器就是含有`yield`语句的函数。

`yield`语句放弃cpu，保留当前上下文，并将参数作为next()的返回值，直到被调用才继续执行。

生成器通过next()和send(None)唤醒，send(None)会执行yield但不继续往下执行，可以用于**执行流程的控制**。

```python
def fib():
    prev, curr = 0, 1
    while True:
        yield curr
        prev, curr = curr, prev+curr
g = fib()
for i in g:
    print i

def f():
    print (yield),
    print 0,
    print (yield),
    print 1

g = f()
# TypeError: can't send non-None value to a just-started generator
# g.send(None) or next(g)
# When send() is called to start the generator, it must be called with None as the argument, because there is no yield expression that could receive the value.
g.send('x')
g.send('y')
```
# 表达式生成器
square = [x\*x for x in range(10)]
square = list(x\*x for x in range(10))

```python
## 主要思想Lazy Factory
```

# 函数装饰器

非侵入式给函数增加功能，**控制函数执行流程**，类似AOP面向切面编程。

Python中可将函数作为参数、返回值，以及在函数里定义函数。

装饰器函数接受函数为参数，返回包装后的函数。

```python
def deco(func):
    def wrapper_func(*args, **kwargs):
        before...
        func(*args, **kwargs)
        after...
    return wrapper_func

foo = deco(foo)
foo()
# 语法糖 @
@deco
def foo():
    pass
```

除函数参数外其他参数：

```python
def use_logging(level):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if level == "warning":
                logging.warn("xx")
               return func(*args, **kwargs)
        return wrapper
    return decorator
```

使用元信息：

```python
from functools import wraps

def decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        print func.__name__
        return func(*args, **kwargs)
    return decorator
```

执行顺序：

```python
@a
@b
@c
def f():
    pass
f = a(b(c(f)))
```



# 类装饰器



可以使用实现\_\_call\_\_方法的类装饰函数。主要用在继承的场景，可以给装饰器添加新功能。

```python
class Foo(object):
    def __init__(self, func):
        self._func = func
        
    def __call__(self):
        before...
        self._func()
        after...
@Foo
def f():
    pass
```

# 描述符(Descriptor)

通过对类的操作进行hook，来控制其行为。

描述符类中实现\_\_get\_\_()，\_\_set\_\_()，和\_\_delete\_\_()中一或多个。

描述符对象需要赋值给其他类的一个属性。



### python可以通过@property来控制类属性

```python
class Foo(object):
    self._attr = attr
    
    @property
    def attr(self):
        return self._attr
    @attr.setter
    def attr(self, value):
        xxx
```

### python属性访问默认顺序，以obj.x为例：

1. obj.\_\_dict\_\_['x']
2. type(obj).\_\_dict\_\_['x']
3. type(obj)除元类以外的基类
4. 如果值为包含一个描述方法的对象，python会调用该方法，比如\_\_get\_\_()，类需继承自type或object

### 描述符协议

```python
desc.__get__(self, obj, type=None) --> value
desc.__set__(self, obj, value) --> None
desc.__delete__(self, obj) --> None
```

1. **资料描述符(data descriptor)**：同时定义\_\_get\_\_()和\_\_set\_\_()方法，实例字典中有同名优先访问资料描述符；

2. **非资料描述符**：只定义了\_\_get\_\_()方法，实例字典中有同名项优先访问字典项；

   x.\_\_get\_\_(obj)：

   1. **obj为对象**，`object.__getattribute__()`，将`b.x`转换为`type(b).__dict__['x'].__get__(b, type(b))`
   2. **obj为类**，`type.__getattribute__()`，将`B.x`转换为`B.__dict__['x'].__get__(None, B)`

   *更多详细真实内容参考cpython实现。*

   *可通过在\_\_set\_\_()方法中抛出AttributeError构造只读资料描述符。*

3. **实例**：属性、绑定和非绑定方法、静态方法、类方法等。

https://harveyqing.gitbooks.io/python-read-and-write/content/python_advance/python_descriptor.html

https://zhuanlan.zhihu.com/p/32764345