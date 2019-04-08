# 设计模式
Classes should be open for extension, but closed be modification.
扩展就是组合，不改变任何已有代码。

## 装饰模式(Decorator)
主要为了动态给对象添加额外的职责(behavior)和功能，不改变其数据结构，比如Java IO中BufferedInputStream就是一个装饰器，改变了read的行为，但是没有改变InputStream实际类的结构。继承子类会爆炸增长。
两个例子：  
1. 房子装修
    - 装地板、粉刷墙等，房子结构未变，修饰了一下。
2. 山东大饼
    - 加青菜、加培根、加鸡蛋等，改变数据内容，但是不改结构。// cooked

基本模式：  
一个抽象接口Component  
一个实现类ConcreteComponent  
一个装饰器基类Decorator  
多个不同装饰器子类ConcreteDecorator

```java
// Room.java
public interface Room {
    public String showRoom();
}
// BlankRoom.java
public class BlankRoom implements Room {
    public String showRoom(){
        return "毛坯房";
    }
}
// RoomDecorator.java
public class RoomDecorator implements Room {
    private Room roomToBeDecorated;
    public RoomDecorator(Room room){
        this.roomToBeDecorated = room;
    }
    public String showRoom(){
        return roomToBeDecorated.showRoom();
    }
}
// PaintedDecorator.java
public class PaintedDecorator extends RoomDecorator {
    public PaintedDecorator(Room room){
        super(room);
    }
    public String showRoom(){
        doPaiting();
        return super.showRoom()+"粉刷墙";
    }
}
// FlooredDecorator.java
public class  FlooredDecorator extends RoomDecorator {
    public FlooredDecorator(Room room){
        super(room);
    }
    public String showRoom(){
        doPaiting();
        return super.showRoom()+"铺地板";
    }
}
public static void main(String[] args){
    Room blankRoom = new BlankRoom();
    Room paintedRoom = new PaintedDecorator(blankRoom)
    Room paintedAndFlooredRoom = new FlooredDecorator(new PaintedDecorator(blankRoom));
    System.out.println(blankRoom.showRoom());
    System.out.println(paintedRoom.showRoom());
    System.out.println(paintedAndFlooredRoom.showRoom());
}
```

https://www.ibm.com/developerworks/cn/java/j-lo-cdi-decorator-pattern/index.html