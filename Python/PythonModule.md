# Python模块

一种代码组织形式。可包含可执行代码、函数、类等。
模块名称，\_\_name\_\_

## 对比
||文件系统|Python|
|:-:|:-:|:-:|
||文件|模块|
||目录|包|

## 单模块组织
```python
# /usr/bin/env python
"this is the doc of the module"
import sys
class Foo(object):
    "this is the doc of class."
    def func(self):
        "this is the doc of function."
        pass
if __name__ == "__main__":
    foo = Foo()
    foo.func()
```
