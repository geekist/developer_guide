
# 代理模式

## 定义

所谓的代理，就是一个人或者一个机构代表另外一个人或者另外一个机构采取行动。在一些情况下，一个客户不想或者不能够直接引用一个对象，而代理对象可以在客户端和目标对象中间起到中介的作用。

代理模式的实现有静态代理和动态代理两种。

## 场景 

![](https://upload-images.jianshu.io/upload_images/5408072-8e0c5ee4f8437d3f.png)

代理模式中的角色有：

抽象对象角色(AbstractObject)：声明了目标对象和代理对象的共同接口，这样依赖在任何可以使用目标对象的地方都可以使用代理对象。
目标对象角色(RealObject)：定义了代理对象所代表的目标对象。
代理对象角色(ProxyObject)：代理对象内部含有目标对象的引用，从而可以在任何时候操作目标对象；代理对象提供一个与目标对象相同的接口，以便可以在任何时候替代目标对象。代理对象通常在客户端调用传递给目标对象之前或者之后，执行某个操作，而不是单纯的将调用传递给目标对象。


## 实例
```java

// 共同接口 //
public interface ISubject {
    void doAction();
    void byebye();
}

// 真实对象(委托类) //
public class RealSubject implements ISubject {
    @Override
    public void doAction() { System.out.println("Real Action Here!"); }
    @Override
    public void byebye() { System.out.println("Wave goodbye!"); }
}

// 代理对象(代理类) //
public class SubjectProxy implements ISubject {
    private ISubject subject;

    public SubjectProxy() {
        // RealSubject实例可根据环境变量、配置等创建不同类型的实例(多态)
        subject = new RealSubject(); // 此处仅简单地new实例
    }

    @Override
    public void doAction() {
        System.out.println(">> doWhatever start"); // 扩展进行额外的功能操作(如鉴权、计时、日志等)
        subject.doAction();
        System.out.println("doWhatever end <<");   // 扩展进行额外的功能操作(如鉴权、计时、日志等)
    }
    @Override
    public void byebye() {
        System.out.println("Say goodbye"); // 改变委托类行为(例如实现数据库连接池时避免close关闭连接)
    }
}

// 验证代码 //
public class StaticProxyDemo {
    public static void main(String[] args) {
        SubjectProxy subject = new SubjectProxy();
        subject.doAction();
        subject.byebye();
    }
}

```
