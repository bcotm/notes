## 注解(Annotation)
Java中类、方法、变量、参数和包的一种 **元数据**。   
可通过反射获取注解内容，即编译时被嵌入到字节码中，运行时加载到JVM里。   

### 定义
注解是对Java中事物的一种解释，类似一张标签。  
**元注解**可以对注解进行注解(解释)。   
**\@Retention**: 注解存活时间，RetentionPolicy.SOURCE, CLASS, RUNTIME   
**\@Documented**:   
**\@Target**: 注解使用地方，ElementType.ANNOTATION_TYPE, CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE(类、接口、枚举)   
**\@Inherited**: 子类继承超类注解   
**\@Repeatable**: 注解被多次使用

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {

}
// 容器注解
@interface Persons {
    Person[] value();
}
@Repeatable(Persons.class)
public @interface Person {
    String role default "";
}

@Person(role = "coder")
@Person(role = "artist")
@TestAnnotation
public class AnnotatedClass {

}
```

### 注解的属性
#### 属性的类型
基本数据类型、类、接口、注解以及对应的数组。   

```java
@Retention(RetentionPolicy.RUNTIME)
public @interface TestAnnotation {
    int id() default -1;
    String msg() default "";
}
@TestAnnotation(id=2, msg="hahaha")
public class Test {}
```

### Java内置注解
**\@Deprecated**   
标记过时的元素，编译器会告警   
**\@Override**   
提示子类重写父类中需要重写的方法   
**\@SuppressWarnings**   
忽略编译器的告警   
**\@SafeVarargs**   
**\@FunctionalInterface**   
函数式接口注解   

### 注解的使用
```java
@TestAnnotation(msg="hello")
public class Test {
    
    @Check(value="hi")
    int a;
    
    
    @Perform
    public void testMethod(){}
    
    
    @SuppressWarnings("deprecation")
    public void test1(){
        Hero hero = new Hero();
        hero.say();
        hero.speak();
    }


    public static void main(String[] args) {
        
        boolean hasAnnotation = Test.class.isAnnotationPresent(TestAnnotation.class);
        
        if ( hasAnnotation ) {
            TestAnnotation testAnnotation = Test.class.getAnnotation(TestAnnotation.class);
            //获取类的注解
            System.out.println("id:"+testAnnotation.id());
            System.out.println("msg:"+testAnnotation.msg());
        }
        
        
        try {
            Field a = Test.class.getDeclaredField("a");
            a.setAccessible(true);
            //获取一个成员变量上的注解
            Check check = a.getAnnotation(Check.class);
            
            if ( check != null ) {
                System.out.println("check value:"+check.value());
            }
            
            Method testMethod = Test.class.getDeclaredMethod("testMethod");
            
            if ( testMethod != null ) {
                // 获取方法中的注解
                Annotation[] ans = testMethod.getAnnotations();
                for( int i = 0;i < ans.length;i++) {
                    System.out.println("method testMethod annotation:"+ans[i].annotationType().getSimpleName());
                }
            }
        } catch (NoSuchFieldException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            System.out.println(e.getMessage());
        } catch (SecurityException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            System.out.println(e.getMessage());
        } catch (NoSuchMethodException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
            System.out.println(e.getMessage());
        }
        
        

    }

}
```

### 注解的作用
给编译器提供信息，探测错误、警告，生成代码、文档等   
给开发者提供信息，APT(Annotation Processing Tool)   
实例：   
1. 测试框架，被测试代码带上一定注解，测试框架来使用   
2. IOC框架，依赖注入   