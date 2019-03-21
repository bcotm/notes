# Java Interface(Java接口)

## Comparable
```java
int compareTo();
```

## Cloneable(标记接口)
默认是浅拷贝。
```java
protected Object clone();
```
通过实现Cloneable接口实现深拷贝。

## Callback(回调)
调用者知道调用哪一个方法，通过接口声明，被调者实现该接口。

```java
public interface ActionListener{
    void actionPerformed();
}

class Callee implements ActionListener{
    public void actionPerformed(){
        System.out.println("I'm called!!");
    }
}
```