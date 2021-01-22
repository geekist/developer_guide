# 策略模式

## 定义

在开发中常常遇到这种情况，实现某一个功能有多方式，我们可以根据不同的条件选择不同的方式来完成该功能。最常用的方法是将这些算法方式写到一个类中，在该类中提供多个方法，每一个方法对应一个具体的算法；或者通过if…else…或者case等条件判断语句来进行选择。

然而该类代码将较复杂，维护较为困难。如果我们把一个类中经常改变或者将来可能改变的部分提取出来，作为一个接口，然后在类中包含这个对象的实例，这样类的实例在运行时就可以随意调用实现了这个接口的类的行为。这就是策略模式。

## 场景

## 实例


我们直接来看例子：

1.策略接口

```java

public interface Strategy {
    void testStrategy();
}

```
2.准备两个实现类

```java

public class StrategyA implements Strategy {
    @Override
    public void testStrategy() {
        System.out.println("我是实现类A");
    }
}

public class StrategyB implements Strategy {
    @Override
    public void testStrategy() {
        System.out.println("我是实现类B");
    }
}
```
3.策略执行Context类

```java

public class Context {
    
    private Strategy stg;
    
    public void doAction() {
        this.stg.testStrategy();
    }
    /*  Getter And Setter */
    public Strategy getStg() {
        return stg;
    }

    public void setStg(Strategy stg) {
        this.stg = stg;
    }
}
```

这时候我们准备一个main方法来测试一下他
```java
public class StrategyTest {
    public static void main(String[] args) {
        Strategy stgB = new StrategyB();
        Context context = new Context(stgB);
        context.setStg(stgB);
        context.doAction();
    }
}

```

## state和strategy的区别
strategy的抽象主要是算法的抽象，state侧重的是对状态的抽象