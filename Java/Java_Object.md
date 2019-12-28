## java.lang.Object

```java
public boolean equals(Object obj){ return (this == obja); }//原本Object.java中的实现是判断地址是否相同。
public int hashCode()//默认是对象地址？
```
**equals()**更像逻辑定义对象是否相同，**hashCode()**则是实际的散列值，**==**计算操作数的值之间的关系(引用型变量则是内存中的地址)。  

### `equals()`：  
判断两个对象是否(内容)相同：  

1. 自反性，自己跟自己相同。
2. 对称性，A和B相同，B和A也相同。
3. 传递性，A和B相同，B和C相同，A和C也相同。
4. 一致性，运行时，A和B无内容更改则一直相同。

String、Math、Integer等封装类已经Override了equals()方法。  
比如，String类的equals()可能实现：  
```java
// 1. 引用地址； 2. 对象类型； 3. 内容。
public boolean equals(Object other){
    if(other == null){
        return false;
    }
    if(this == other){
        return true;
    }
    if(other instanceof String){
        String another = (String) other;
        // 对比详细字符内容
    }
    return false;
}
```

### `hashCode()`:  
被用于hash tables，在不允许重复的集合中采用哈希表方式来判断对象是否相同。  
**首先判断hashCode是否相同，再判断equals。如果hashCode不同，则跳过equals判断。**  

1. 多次调用相同。
2. **equals()相同，hashCode()必须相同。**否则根据散列算法，两个不同哈希值的对象在实际逻辑定义上是相同的(equals())。


### 在对象使用跟重复有关的集合类时，一定注意重载`equals()`和`hashCode()`方法。
在集合中添加对象时，如果不重新定义hashCode，两个equals相同的不同对象会被当做两个对象。



#### 对比对象不属于同一个类

```java
public boolean equals(Object otherObj){
    // 第一种
    if(getClass() != otherObj.getClass()){
        return false;
    }
    // 第二种，所有子类有同样的语义
    if(!(otherObj instanceof ClassName)){
        return false;
    }
}
```



对象通过 + 号连接时，默认调用 toString()，一般为类名+属性值

Arrays.toString(arr);Arrays.deepToString(twoDimArr);



## 实现类克隆

implements Cloneable

