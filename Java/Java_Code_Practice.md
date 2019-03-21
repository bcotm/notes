[Java Tutorial Doc](https://docs.oracle.com/javase/tutorial/)
# 概念设计相关
## 修饰符(Modifiers)
### 访问修饰符(Access modifier)
* **default**: *package*
* **public**: *everything*
* **private**: *inside class(object)*
* **protected**: *subclass and package*

### 非访问修饰符(Non-access modifier)
* **final**: *finalizing the implementation of classes, methods and variables.*
* **abstract**
* **static**
* **synchronized**
* **volatile**

#### Static
主要修饰：变量、方法、代码块、内部类、内部接口、静态导入包；   
类加载期间，静态变量被赋值，与静态方法存到方法区，静态代码段被执行且**只被执行一次**；
```java
public class StaticTest {
    private static int number = 1; // if not defined, default value;
    public static final String CONSTANT = "constant variable";
    static {
        System.out.println("This is a static blcock which has being executed during class loading.");
    }
    public static class InnerStaticClass {
        public static void whatMethod(){
            System.out.println("InnerStaticClass.whatMethod has been called.");
        }
    }
    public static interface InnerStaticInterface {
        public void IT();
        public static void ITS(){};
    }
    public static void main(String[] args){
        System.out.println("This is static method main.");
        InnerStaticClass.whatMethod();
    }
}
```
## 变量类型(Variable Types)
- **Local**: belong to **methods, blocks, constructors**; stack, no default   
*栈中数据可以共享，比如int a=3;int b=3;*   
*栈中创建a引用，查字面值为3的地址，没有就开辟内存，有就a指向该地址;*   
*字面值在常量池，在方法区；*   
- **Static**: belong to a **class**(class level); heap 
- **Instance**: belong to an **object**(object level); heap

## 数据类型(Data Types)
### 基础类型(Primitive Type)
* byte
* short
* int
* long
* float
* double
* boolean
* char

### 引用类型(Reference Type)
- Objects(Instance)
```java
dataType objReference = new dataType();
```

- Arrays
```java
dataType[] arrayRefVar = new dataType[arraySize];
```
array.length
- Enum
```java
enum FreshJuiceSize{ SMALL, MEDIUM, LARGE}
FreshJuiceSize fjSize;
```

### 常量(Literals)
68, 'a', 0x64, "Hello World", '\u0001', 

## 类(Class)
### 构造器(Constructor)
三种类型
```java
public class ThreeConstructors {
    ThreeConstructors(){} //Void Constructor
    ThreeConstructors(ThreeConstructors three){} //Parameterized Constructors;Copy constructor is used in assignment.
    // Default Constructor is which writtened by compiler when no any constructor being defined in the class body.
    // Where is the third one?
}
```
构造函数顺序   
```java
public class ConstructorSequence {
    ConstructorSequence(){
        super();
        this();
        // otherMemberObject(); 
    }
}
```
构造链
```java
public class ChainedConstructor {
    private String name;
    // using void constructor to invoke parameterized constructor.
    ChainedConstructor(){
        this("Call Parameterized Constructor.Set name to default value.")
    }
    ChainedConstructor(String str){
        this(str, 18);
    }
    ChainedConstructor(String str, int i){
        this.name = str;
        this.age = i;
    }
    public static void main(String[] args){
        ChainedConstructor cc = new ChainedConstructor();
    }
}
```

### 继承


### 包(package)
1. package包含class, interface, sub-packages;

# 代码语法相关
## 源码组织
### 源码文件
1. 文件名以**.java**结尾
2. 文件名与**唯一**public类名相同;
3. package语句处于第一行;
4. import语句在类定义之前;

## 操作符(Operators)
### 算数(Arithmetic Operators)
+-*/%,++--
### 关系(Relational Operators)
==, !=, >, <, >=, <=
### 位(Bitwise Operators)
&, |, ^, ~, <<, >>, >>>
### 逻辑(Logical Operators)
&&, ||, !
### 赋值(Assignment Operators)
=, +=, -=...

## 循环(Loop)
if condition is true, loop conditional code   
while, do while, for
break, continue
## 条件判断(Decision)
if, if else, switch, ?:

## 数字类型(Number)包装类Wrapper
java.lang

**Byte, Short, Integer, Long, Float, Double**
编译器自动装箱就拆箱

### 常用方法
1. **与基础数据类型转换**: xxValue(), Wrapper(primitive type), Wrapper.valueOf(String s, int radix)
2. **转为字符串类型**: Wrapper.toString(primitive type)
3. **整型还有无符号、位操作相关工具函数**: Wrapper.toUnsignedInt(primitive type), Integer.highestOneBit(int i)

## 字符类型(Charactor)和字符串类型(String)
char是Unicode(包括字符数组、String、StringBuilder)，java采用UTF-16，基本平面16位(U+0000, U+FFFF)   
String是char列表组成的类，immutable，     
> 字符编码问题详见《字符编码》

### 常用方法
#### char
大小写、空白符等

#### String
1. **与其他类型互换**: byte[], char[], int[], StringBuffer, StringBuilder, getBytes()/\*与系统编码有关\*/ //代码点、字节、代码单元等
2. **代码点、代码单元处理**： charAt, codePointAt(), codePointCount(bidx, eidx)
3. **多字符串处理**: 比较、连接等，compareTo, concat, contains, 
4. **单字符串处理**: 搜索、提取等，endsWith, format, indexOf, isEmpty, length, replace, split, subSequence

#### StringBuilder
mutable, not synchronized   
有容量，会自动扩容capacity(), length()   
可以修改内容，append(), insert(), delete(), reverse()   


## 数组(Array)
### 常用方法
Arrays.xxx：排序、搜索、填充等


## 异常
问题可能由 用户、程序员、物理资源导致。
**Throwable**是java所有错误、异常的父类。JVM会抛、throw能抛、catch能捕获。包含了创建是线程的**执行栈快照**。*cause*是造成该Throwable的另一个Throwable-->异常链。   
> cause的原因：   
> 1. 不用将底层异常一直往外抛，这会使上层实现与底层耦合。   
> 2. 可以不更改上层异常集的情况下，将故障详细信息传递给调用者。使实现可以通过包装异常满足特定接口。   

**Error**是指程序不应该捕获的问题。内部错误或者资源耗尽。程序几乎肯定无法恢复。*Unchecked*。   
>AssertionError: 断言错误。   
>LinkageError: 类在编译后所依赖的类发生了不兼容变化。   
>VirtualMachineError: VM出问题了或者没资源了。
>IOError: I/O时严重问题。      

**Exception**是指程序应该捕获的问题。*checked*异常需要在方法定义时throws抛出。   

**checked** 与 **unchecked** 区别在与编译时期是否检查该问题。unchecked异常RuntimException是程序员自身导致的。对于checked问题，程序员必要处理，声明或者捕捉，throws或者catch，它们以某种方式依赖程序之外的外部因素。比如 **IOException** 

最好把异常集控制在一定范围内。
捕获异常：在****发生异常时的调用栈中依次往回搜索合适的异常处理器****，都找不到则运行时终止。
finally会在try catch语句return之前执行，以下四种情况不执行：
1. finally中发生异常;
2. 前面代码有System.exit();
3. 程序所在线程死亡;
4. 关闭CPU。

一般处理问题：约定错误码、抛出异常。
自定义异常继承Exception和RuntimeException分别是checked和unchecked异常。

可恢复的错误就用checked异常。
1. 编译器强制检查，避免不处理
2. 不声明异常调用者难处理

1. 一直延调用站往上抛会破坏顶层设计，耦合 -- 合理范围内catch住？--然后包装异常后抛出，统一抛出ApplicationException类似
或者统一基类异常
2. 接口难改变 -- 接口不适用checked异常
3. 即使有声明了checked异常，方法也可能使用unchecked异常
