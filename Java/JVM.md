## JVM
### Class Loader
Java程序运行，从类加载器开始。
类加载器又分为三个阶段，**加载(Loading)**, **链接(Linking)**, **初始化(Initializing)**   

#### 加载(Loading)
主要有三个Loader加载不同的资源，定位类文件并将其字节流装载到JVM中。   
**Bootstrap Class Loader** 加载 **rt.jar** 以及 **JAVA_HOME/lib** 目录下的jar包。   
**Extention Class Loader** 加载 **JAVA_HOME/lib/ext**目录下的jar包。   
**System Class Loader** 加载 **classpath**目录下的jar包。   
>java -Xbootclasspath   
-Djava.ext.dirs
-Djava.class.path   

#### 链接(Linking)
加载进来的字节流需要分配内存结构保存其信息，Runtime Data Areas  
此时数据还不可用

1. **验证** 字节码的格式以及安全性等；   
2. **内存分配** 为类准备内存空间，并设置static variables 为 默认值   
3. **解析** 加载类所引用的其他类、接口等，符号引用改为实际引用   

#### 初始化(Initializing)
给**static variables**赋值   
执行**static blocks**   

### Runtime Data Areas
JVM中内存空间布局逻辑。   
主要分为线程共用和线程私有的区域。   
**线程私有区域**： 程序计数器、执行栈。   
**线程共用区域**： 方法区、堆、本地方法执行栈。   

**方法区**： 运行时常量池、类元数据、静态变量。   
**堆**： 对象、实例变量、数组等。   

### Execution Engine