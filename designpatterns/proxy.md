
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

## java 动态代理

动态代理利用了JDK API，动态地在内存中构建代理对象，从而实现对目标对象的代理功能。动态代理又被称为JDK代理或接口代理。

静态代理与动态代理的区别主要在：

静态代理在编译时就已经实现，编译完成后代理类是一个实际的class文件
动态代理是在运行时动态生成的，即编译完成后没有实际的class文件，而是在运行时动态生成类字节码，并加载到JVM中

特点：

动态代理对象不需要实现接口，但是要求目标对象必须实现接口，否则不能使用动态代理。

JDK中生成代理对象主要涉及的类有

* java.lang.reflect Proxy

主要方法为

```java
static Object  newProxyInstance(ClassLoader loader,  //指定当前目标对象使用类加载器
                                Class<?>[] interfaces,    //目标对象实现的接口的类型
                               InvocationHandler h      //事件处理器
) 
//返回一个指定接口的代理类实例，该接口可以将方法调用指派到指定的调用处理程序。
```
* java.lang.reflect InvocationHandler

```java
 Object    invoke(Object proxy, Method method, Object[] args) 
// 在代理实例上处理方法调用并返回结果。
```
>举例：保存用户功能的动态代理实现


接口类： IUserDao

```java
package com.proxy;

public interface IUserDao {
    public void save();
}
```

目标对象：UserDao

```java
package com.proxy;

public class UserDao implements IUserDao{
    @Override
    public void save() {
        System.out.println("保存数据");
    }
}
```

动态代理对象：UserProxyFactory
```java
package com.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class ProxyFactory {

    private Object target;// 维护一个目标对象

    public ProxyFactory(Object target) {
        this.target = target;
    }

    // 为目标对象生成代理对象
    public Object getProxyInstance() {
        return Proxy.newProxyInstance(

           target.getClass().getClassLoader(), 

           target.getClass().getInterfaces(),
                
           new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("开启事务");

                        // 执行目标对象方法
                        Object returnValue = method.invoke(target, args);

                        System.out.println("提交事务");
                        return null;
                    }
                });
    }
}


```

```java
测试类：TestProxy

package com.proxy;

import org.junit.Test;

public class TestProxy {

    @Test
    public void testDynamicProxy (){
        IUserDao target = new UserDao();
        System.out.println(target.getClass());  //输出目标对象信息
        IUserDao proxy = (IUserDao) new ProxyFactory(target).getProxyInstance();
        System.out.println(proxy.getClass());  //输出代理对象信息
        proxy.save();  //执行代理方法
    }
}


```
