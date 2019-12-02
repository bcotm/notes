## Java Object Oriented
### 概念
**对象(Object)**是一个实例，有状态和行为。   
**类(Class)**是对象的抽象(模板、蓝图)，描述一类对象的状态和行为，也即变量和方法。   

### 特性
#### 封装(Encapsulation)
将状态和行为(**fields**, **methods**)与对象绑定在一起。   
并通过 **访问修饰符** 达到访问控制的目的。   

* **default**: *package，同一包内均可访问。*   
* **public**: *everything，任何地方均可访问。*   
* **private**: *inside class(object)，仅在对象内可以访问。*   
* **protected**: *subclass and package，子类和同一包下可以访问。*   
> 继承后不能缩小**访问修饰符**范围，即parent public，child不能private。   
> 继承后不能扩大所抛**异常**范围，即parent NullPointerException，child不能Exception。   

#### 继承(Inheritance)
继承是一种 **is-a** 关系，子类可重用父类的字段和方法。   
Java只允许单继承，即一个类只能允许最多有一个父类。   
Java中可以通过 **接口(interface)** 实现多继承。   

**继承类型**   

1. 单继承 ： class DerivedClass extends BaseClass {}   
2. 多继承 ： class SonClass implements FatherInterface {}   
3. 多级继承 ： class GrandsonClass extends SonClass {}   

**动态绑定**

虚编译器不能准确知道调用方法，通过方法表确定，内联 。

#### 多态(Polymorphism)
performs a single action in different ways.     

**实现方式**   
1. 方法重载(Overloading)。静态、编译时绑定。
相同方法名，不同参数个数、参数类型、*参数顺序*。   
2. 方法重写(Overriding)。动态、方法调度。dynamic method dispath。
子类重写父类同名同参方法。   

#### 抽象(Abstraction)
作用于 **方法** 、 **类** 和 **接口** 上，没有实现，不能被实例化。   
class partial abstract   
interface full abstract   
继承抽象类必须实现，否则类继续抽象。   

```java
abstract class AbstractClass{
    public abstract String abstractMethod();
    public String concreteMethod(){
        // impl
	}
}
```



### 构造器(Constructor)
在 **创建对象实例** 时调用的方法，初始化对象。   

#### 构造器类型
1. **Default**： 编译器会在第一行插入super()。   
2. **No-arg**： 可以调用super(parameter)。   
3. **Paramiterized**： 同上。   

#### 构造器链(Constructor Chaining)
```java
public class Wow {
    Wow(){
        this("wow");
    }
    Wow(String str){
        this(1, str);
    }
    Wow(int i, String str){
        //xxxx
    }
}
```

#### Private Constructor
可以用来实现单例，防止类以外创建实例。   
```java
class SingletonExample {
    private static SingletonExample singletonExampleObj = null;
    // Look at here !!! PRIVATE constructor !!!
    private SingletonExample(){}
    public static SingletonExample getSingletonExampleObject(){
        if(singletonExampleObj == null){
            singletonExampleObj = new SingletonExample();
        }
        return singletonExampleObj;
    }
    
    public void disp(){
        System.out.println("disp() method called");
    }
}

public class OutsideClass {
    public static void main(String args[]){
        //Obtaining the object via our public method
        SingletonExample ss = SingletonExample.getSingletonExampleObject();
        ss.disp();
    }
}
```

### 接口(interface)
完全抽象。   
public abstract methods with no body   
public static final fields   

#### 接口、类之间的关系
> 类与类之间，单继承。class A extends B {}   
> 类与接口，多实现。class A implements B, C, D {}  
> - 接口之间可能存在同名不同返回值类型，会导致只能实现其中一个，因为重载不了哇。    
> 接口与接口，单继承。interface A extends B {}   

#### 空接口(标记接口，Marker Interface)
没有变量和方法，比如Serializable, Cloneable, Remote, RandomAccess, EventListener等。   
Java 1.5之后被Annotation替代。   