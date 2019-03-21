# Java Generic
泛型本质是**参数化类型**。
1. 只在**编译时期**有效。
2. 正确检测之后会将泛型相关信息**擦除**。
3. 在对象进入、离开方法的边界处添加类型检查和**类型转换**。

- 虚拟机中没有泛型，只有普通类。
- 所有类型参数用他们的限定类型替换，默认Object。
- 桥方法被合成来保持多态。
- 运行时的类型检查只看原始类型，比如Pair<String> 和 Pari<Double> 的getClass()都返回 Pair.class。
- 不能捕获、抛出泛型类实例。不能用数组。

## 泛型类和接口
```java
// 泛型类
public class GenericClass<T> {
    private T mem = null;
}

public interface GI<T> {
    public T next();
}

public class GC implements GI<String>{
    @Override
    public String next(){}  
}
```

## 泛型方法
```java
// 泛型方法，也可以存在于普通类中
public class NormarlClass {
    public static <T> T getGenericMethod(){}

    //...调用
    NormarlClass.<String>getGenericMethod();

    // 限制参数类型，默认第一个为原始类型
    public static <T extends Comparable & Serializable> T min(T[] a){}

    // 通配符参数类型
    public static <?> Pair<? extends Employee> getOne(){}
}


```

## 通配符及上下界
```java
// 任意类型
public class ArrayList<?>{}
// E及其子类
public class ArrayList<? extends E>{}
// E及其父类
public class ArrayList<? super E>{}
```

## 问题
1. 通配符? var，无法声明成员变量。
2. 类型擦除导致T无法new，类型无法判断。工厂模式、class。
    ```java
    class Foo { 
     public void doSomething(Set<?> set) { 
       Set<?> copy = new HashSet<Object>(set);  
        } 
    }
    ```
3. 泛型不能协变。https://www.ibm.com/developerworks/cn/java/j-jtp01255.html