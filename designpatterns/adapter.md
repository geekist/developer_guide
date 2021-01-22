
# 适配器模式


## 定义

适配器模式，即定义一个包装类，用于包装不兼容接口的对象

把一个类的接口变换成客户端所期待的另一种接口，从而使原本接口不匹配而无法一起工作的两个类能够在一起工作。

如：不同国家的电源插头和插座，需要一个适配器来协调工作。

适配器模式的形式分为：类的适配器模式和对象的适配器模式

## 场景
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0yNGM2YmY0NGRhMWI3OWFkLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

## 实例

步骤1： 创建Target接口；
```java
public interface Target {
 
    //这是源类Adapteee没有的方法
    public void Request(); 
}
```

步骤2： 创建源类（Adaptee） ；

```java
public class Adaptee {  
    public void SpecificRequest(){
    }
}
```

步骤3： 创建适配器类（Adapter）

```java
//适配器Adapter继承自Adaptee，同时又实现了目标(Target)接口。
public class Adapter extends Adaptee implements Target {

    //目标接口要求调用Request()这个方法名，但源类Adaptee没有方法Request()
    //因此适配器补充上这个方法名
    //但实际上Request()只是调用源类Adaptee的SpecificRequest()方法的内容
    //所以适配器只是将SpecificRequest()方法作了一层封装，封装成Target可以调用的Request()而已
    @Override
    public void Request() {
        this.SpecificRequest();
    }

}
```
调用

```java
public class AdapterPattern {

    public static void main(String[] args){

        Target mAdapter = new Adapter()；
        mAdapter.Request（）;
     
    }
}

```