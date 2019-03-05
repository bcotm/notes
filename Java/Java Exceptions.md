## 异常
感觉在工具类之类的把checked exception控制住，然后逻辑层使用的时候不考虑这些，要么全都转化成unchecked抛出，AppBaseException
问题可能由 用户、程序员、物理资源导致。   
方法失败的两个基本原因，思考异常条件的本质。
设计各层之间的通信，包括异常通信。

异常处理方法：
1. 捕获
2. 记录
3. 尝试确定发生了什么 -- 如果调用者需要大量逻辑来确定某个操作何时、为何失败，说明组件错误报告的设计很糟糕。
4. 将一个异常转换成另一个异常

事后处理异常（或者根本不处理）是项目混乱的主要原因。

java主要分为 Error， checked Exception，Unchecked Exception(RuntimeException)

Checked:
java.io 抛出 IOException
失败在于Java API设计人员认为大多数失败条件是相同的，可以通过相同类型的异常进行通信。
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
