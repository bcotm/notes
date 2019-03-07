# Java Reflect
## Class对象
获取Class对象。

1. `o.getClass()`
2. `o.class`
3. `Class.forName()`

## java.lang.reflect
获取类或对象的信息

1. `getMethods("methodName", int.class, String.class)`
2. `getDeclaredFields()`

## AccessObject
设置访问控制符。
1. `AccessObject.setAccessible(fields, true);`
2. `f.setAccessible(true);`

## 方法指针
调用方法。
`methd.invoke(obj)`