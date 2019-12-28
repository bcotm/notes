# Java Inner Class(Java内部类)

作用：
1. 内部类可访问定义所在作用域的数据。通过指针OuterClass.this.引用外部类数据。外部通过this.new生成。
2. 对同一包的其他类隐藏，可以是私有类。
3. 回调函数匿名内部类。

## 匿名内部类
```java
ActionListener al = new ActionListener(){
    public void actionPerformed(ActionEvent event){}
}
```

## 静态内部类