# Java 注释

为以下内容注释：

1. public **class** **interface**
2. public protected **methods**
3. public protected **fields**

以 **`/**`** 开始，以 **`*/`** 结束。

```java
package com.test.javadoc;

/**
 * 这个是类的注释
 *
 * 这里描述类的详细介绍，跟git commit comment一样
 */
public class JavaDoc {
    /**
     * 这个是方法的注释
     * @author 作者
     * @param paramname 参数的注释
     * @return 返回值说明
     */
    public void whatMethod(String paramname){

    }
}
```