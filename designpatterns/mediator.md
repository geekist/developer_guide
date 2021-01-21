
# 中介模式

## 定义

所谓中介者模式就是用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。

## 场景

![](https://images0.cnblogs.com/blog/381060/201310/01151530-1b6531f07e8f4ec6b66a5cc9da0f776b.png)


## 实例

Mediator: 抽象中介者。定义了同事对象到中介者对象之间的接口。

ConcreteMediator: 具体中介者。实现抽象中介者的方法，它需要知道所有的具体同事类，同时需要从具体的同事类那里接收信息，并且向具体的同事类发送信息。

Colleague: 抽象同事类。

ConcreteColleague: 具体同事类。每个具体同事类都只需要知道自己的行为即可，但是他们都需要认识中介者。

```java
首先是抽象中介者:Mediator.java

public abstract class Mediator {
    //申明一个联络方法
    public abstract void constact(String message,Person person);
}

然后是抽象同事对象:Person.java

public abstract class Person {
    protected String name;
    protected Mediator mediator;
    
    Person(String name,Mediator mediator){
        this.name = name;
        this.mediator = mediator;
    }
    
}

两个具体同事类：HouseOwner.java

public class HouseOwner extends Person{

    HouseOwner(String name, Mediator mediator) {
        super(name, mediator);
    }
    
    /**
     * @desc 与中介者联系
     * @param message
     * @return void
     */
    public void constact(String message){
        mediator.constact(message, this);
    }

    /**
     * @desc 获取信息
     * @param message
     * @return void
     */
    public void getMessage(String message){
        System.out.println("房主:" + name +",获得信息：" + message);
    }
}

Tenant.java

public class Tenant extends Person{
    
    Tenant(String name, Mediator mediator) {
        super(name, mediator);
    }
    
    /**
     * @desc 与中介者联系
     * @param message
     * @return void
     */
    public void constact(String message){
        mediator.constact(message, this);
    }

    /**
     * @desc 获取信息
     * @param message
     * @return void
     */
    public void getMessage(String message){
        System.out.println("租房者:" + name +",获得信息：" + message);
    }
}

具体中介者对象：中介结构、MediatorStructure.java

public class MediatorStructure extends Mediator{
    
//首先中介结构必须知道所有房主和租房者的信息
    private HouseOwner houseOwner;
    private Tenant tenant;

    public HouseOwner getHouseOwner() {
        return houseOwner;
    }

    public void setHouseOwner(HouseOwner houseOwner) {
        this.houseOwner = houseOwner;
    }

    public Tenant getTenant() {
        return tenant;
    }

    public void setTenant(Tenant tenant) {
        this.tenant = tenant;
    }

    public void constact(String message, Person person) {
        if(person == houseOwner){          //如果是房主，则租房者获得信息
            tenant.getMessage(message);
        }
        else{       //反正则是房主获得信息
            houseOwner.getMessage(message);
        }
    }
}

客户端：Client.java

public class Client {
    public static void main(String[] args) {
        //一个房主、一个租房者、一个中介机构
        MediatorStructure mediator = new MediatorStructure();
        
        //房主和租房者只需要知道中介机构即可
        HouseOwner houseOwner = new HouseOwner("张三", mediator);
        Tenant tenant = new Tenant("李四", mediator);
        
        //中介结构要知道房主和租房者
        mediator.setHouseOwner(houseOwner);
        mediator.setTenant(tenant);
        
        tenant.constact("听说你那里有三室的房主出租.....");
        houseOwner.constact("是的!请问你需要租吗?");
    }
}

```