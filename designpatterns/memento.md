# 备忘录模式

## 定义 

所谓备忘录模式就是在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。

## 场景 

![](https://img-blog.csdn.net/20130926211658500?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvY2hlbnNzeQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 实例

Originator: 原发器。负责创建一个备忘录，用以记录当前对象的内部状态，通过也可以使用它来利用备忘录恢复内部状态。同时原发器还可以根据需要决定Memento存储Originator的那些内部状态。

Memento: 备忘录。用于存储Originator的内部状态，并且可以防止Originator以外的对象访问Memento。在备忘录Memento中有两个接口，其中Caretaker只能看到备忘录中的窄接口，它只能将备忘录传递给其他对象。Originator可以看到宽接口，允许它访问返回到先前状态的所有数据。

Caretaker: 负责人。负责保存好备忘录，不能对备忘录的内容进行操作和访问，只能够将备忘录传递给其他对象。

角色类
```java
public class Role{
	private int bloodFlow;
    private int magicPoint;
    
    public Role(int bloodFlow,int magicPoint){
        this.bloodFlow = bloodFlow;
        this.magicPoint = magicPoint;
    }
 
    public int getBloodFlow() {
        return bloodFlow;
    }
 
    public void setBloodFlow(int bloodFlow) {
        this.bloodFlow = bloodFlow;
    }
 
    public int getMagicPoint() {
        return magicPoint;
    }
 
    public void setMagicPoint(int magicPoint) {
        this.magicPoint = magicPoint;
    }
    
    /**
     * @desc 展示角色当前状态
     * @return void
     */
    public void display(){
        System.out.println("用户当前状态:");
        System.out.println("血量:" + getBloodFlow() + ";蓝量:" + getMagicPoint());
    }
    
    /**
     * @desc 保持存档、当前状态
     * @return
     * @return Memento
     */
    public Memento saveMemento(){
        return new Memento(getBloodFlow(), getMagicPoint());
    }
    
    /**
     * @desc 恢复存档
     * @param memento
     * @return void
     */
    public void restoreMemento(Memento memento){
        this.bloodFlow = memento.getBloodFlow();
        this.magicPoint = memento.getMagicPoint();
    }
}


```
备忘录
```java
class Memento {
    private int bloodFlow;
    private int magicPoint;
 
    public int getBloodFlow() {
        return bloodFlow;
    }
 
    public void setBloodFlow(int bloodFlow) {
        this.bloodFlow = bloodFlow;
    }
 
    public int getMagicPoint() {
        return magicPoint;
    }
 
    public void setMagicPoint(int magicPoint) {
        this.magicPoint = magicPoint;
    }
    
    public Memento(int bloodFlow,int magicPoint){
        this.bloodFlow = bloodFlow;
        this.magicPoint = magicPoint;
    }
}

```
负责人
```java
public class Caretaker {
    Memento memento;
 
    public Memento getMemento() {
        return memento;
    }
 
    public void setMemento(Memento memento) {
        this.memento = memento;
    }
 
}

```
客户端
```java
public class Client {
    public static void main(String[] args) {
        //打BOSS之前：血、蓝全部满值
        Role role = new Role(100, 100);
        System.out.println("----------大战BOSS之前----------");
        role.display();
        
        //保持进度
        Caretaker caretaker = new Caretaker();
        caretaker.memento = role.saveMemento();
        
        //大战BOSS，快come Over了
        role.setBloodFlow(20);
        role.setMagicPoint(20);
        System.out.println("----------大战BOSS----------");
        role.display();
        
        //恢复存档
        role.restoreMemento(caretaker.getMemento());
        System.out.println("----------恢复----------");
        role.display();
        
    }
}

```