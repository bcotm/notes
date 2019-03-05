## 包(Package)
### 概念
包将全局单一命名空间分割成各自独立封闭的命名空间。   
包内所有的.class文件被放到同一个目录下，os解决了命名冲突。   
包可用于代码访问权限的控制。   
包内包含子包、类、接口。
> 包 文件夹 命名空间   
源代码 .java文件 编译单元   
package与目录结构需一致；   
class与.java文件名需一致。

### 用法
关键字**package**声明一个包，必须位于源代码文件中非注释第一行，一般为域名倒序。 *比如 com.oracle.java*  
关键字**import**导入一个包，jvm通过一定顺序查找对应文件夹并加载相关信息。java.lang默认导入。

```java
// com/tencent/app/Apple.java
package com.tencent.app;
public class Apple {
    public Apple(){
        System.out.println("com.tencent.app.Apple");
    }
}
// AppleTest.java
import com.tencent.app.Apple;
public class AppleTest {
    public static void main(String[] args) {
        Apple apple = new Apple();
    }
}
```
