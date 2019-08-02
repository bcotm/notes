# Python Import机制

python导入机制很简单，在import的时候，

1. 如果路径(sys.modules)中不存在当前module，则创建一个module对象并执行py文件代码，把py中变量作为module对象的属性。
2. 如果路径(sys.modules)中存在当前module，不做任何事。

### 情形1 

`import x`

``` python
a.py
import b
class Foo:
    pass

b.py
import a
class Bar:
    pass
```

###  情形2 

`from x import y`

```python
a.py
from b import Bar
class Foo:
    pass

b.py
from a import Foo
class Bar:
    pass
```

>  报错原因：
>
> 1. 执行a.py。到`from b` import Bar，创建了`module b`对象，执行b.py代码。
> 2. 执行b.py。到`from a` import Foo，创建了`module a`对象，执行a.py代码。
> 3. 执行a.py。到`from b` import Bar，已经存在`module b`对象，不做任何事，不再执行b.py。
> 4. 继续执行a.py。`from b import Bar`，b.py之前未执行到class Bar，故当前`module b`中不存在该对象，报错。

### 情形3

`from x import *`

```python
a.py
from b import *
class Foo:
    pass

b.py
from a import *
class Bar:
    pass
```

### 导入区别

`import ma`和`from ma import *`

1. 都将`module ma`导入到sys.modules中；
2. from ma import * 将ma中变量名加入到globals()全局命名空间中；