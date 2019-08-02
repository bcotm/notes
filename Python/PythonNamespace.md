# Python命名空间与作用域

## 概念
### 命名空间
**A namespace is a mapping from names to objects.**
实质是一个字典，从名字到对象的映射。

### 命名空间类型及生命周期
1. built-in：内置函数、常量、类型等，解释器创建时创建，计算器退出时销毁。
2. global：模块内函数、类、变量等，模块被import时创建，解释器退出时销毁。
3. local：函数内变量等，函数调用时创建，函数返回时销毁。
4. closure:闭包。


https://segmentfault.com/a/1190000004519811