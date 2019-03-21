## 集合(Collection)
![7d6156954a9971178f947ac7397da222.png](en-resource://database/1068:0)
![c85364a143b232ab6617d4879c3fd221.png](en-resource://database/1070:0)

### 定义
A group of objects, i.e., its elements.

### 构造函数:
**No-arg** Constructor : ()   
**Conversion** Constructor : (Collection c), **shadow copy**

### 方法异常类型:
**"destructive"**: throw UnsupprtedOperationException   
**"optional"**: Operation on ineligible elements: NullPointerException, ClassCastException

### 方法
Most are defined in terms of "**equals()**" method. **compareTo**   
Basic: add, remove, iterator, size, isEmpty, contains
Entire collection: addAll, removeAll, retainAll, containsAll, clear, toArray
> //c.removeAll(Collections.singleton(null)); 
> //immutable set signleton
> //String[] s = c.toArray(new String[0]);

### 遍历
for-each
iterator        //can i.remove()
*aggregate operation*

### 特性:
**重复(Duplicated)**   
**有序(Ordered)**   
**可修改(Modifiable)**   
**空元素(Null Elements)**   
**元素类型(Type of Elements)**   
**同步(Synchronized)**   

### Set Interface
#### No Duplicated
```java
Collection<Type> noDups = new HashSet<Type>(c);
public static <E> Set<E> removeDups(Collection<E> c) {
        return new LinkedHashSet<E>(c);
}
// 并交差 addAll, retainAll, removeAll
```

### List Interface
#### Ordered
Collections.shuffle(list);