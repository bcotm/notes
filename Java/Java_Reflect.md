# Java Reflect
反射：程序可以访问、检测、修改它本身状态和行为的一种能力。

java runtime 维护所有对象的运行时 类型标志，保存则对象所属类的信息。

java.lang.reflect三个类：

1. Field
   1. get/set
2. Method
   1. invoke
3. Constructor
   1. newInstance

获取public域：

o.getClass().getFields();

获取所有声明的方法：

o.getClass().getDeclareMethods();

使用前如果是private需要m.setAccessible(true);

## Class对象

获取Class对象。

1. `o.getClass()`
2. `Class<?> c = XXClass.class`
3. `Class.forName()`，通过完整类名比如`java.util.Date`获取。
4. `Class x = classLoader.loadClass("xx.xx");`

## java.lang.reflect
获取类或对象的信息

1. `getMethod("methodName", int.class, String.class)`
2. `getDeclaredFields()`
3. `getDeclaredConstructor(int.class, int.class)`
4. `getSuperclass();//父类`
5. `getInterfaces();//接口`
6. `getDeclaredClasses();//内部类`

数组扩容需要转为Object后通过java.lang.reflect.Array.newInstance()增加；

## 实例化

1. `c.newInstance();//无参构造`
2. `declaredConstructor.newInstance(xx);//特定构造函数构造`

## AccessObject
设置访问控制符。
1. `AccessObject.setAccessible(fields, true);`
2. `f.setAccessible(true);`

## 方法指针
调用方法。
`method.invoke(obj)`

`method.invoke(null, arg0, arg1)`;//静态方法

## 动态代理接口

* 路由对远程方法的调用；
* 程序运行期间将用户接口事件与动作关联起来；
* 调试跟踪方法；

```java
public class TraceHeandlerDeractor implements InvocationHandler{
    /**
     * 接口动态代理，通过InvocationHandler实现接口调用处理(代理)
     * @param target
     */
    private Object target;

    TraceHeandlerDeractor(Object target){
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println(method + " is called with args." + Arrays.deepToString(args));
        return method.invoke(target, args);
    }
    public static void main(String[] args){
        /**
         * 实际被代理对象（委托对象）和代理对象实现相同接口，通过Proxy生成代理对象；
         */
        Integer value = 10;
        TraceHeandlerDeractor handler = new TraceHeandlerDeractor(value);
        ClassLoader classLoader = value.getClass().getClassLoader();
        Class[] interfaces = value.getClass().getInterfaces();
        Comparable proxy = (Comparable)Proxy.newProxyInstance(classLoader, interfaces, handler);
        System.out.println(proxy.compareTo(10));
    }
}
```

