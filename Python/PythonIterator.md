# Python 迭代器、生成器

## 概念

1. 容器:包含元素的数据结构。
2. 可迭代对象(Iterable):可以返回一个迭代器。
3. 迭代器(Iterator):内部保存了中间状态，可以通过next()返回其所有元素。是一个helper object。
4. 生成器(Generator):简洁的方法生成迭代器，只能被使用一次。通过yield函数返回。
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
# g.send(None)
# When send() is called to start the generator, it must be called with None as the argument, because there is no yield expression that could receive the value.
g.send('x')
g.send('y')
```
# 表达式生成器
square = [x*x for x in range(10)]
square = list(x*x for x in range(10))
```

## 主要思想Lazy Factory

