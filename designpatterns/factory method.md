
# 工厂方法模式

## 简单工厂模式

### 简单工厂定义

将“类实例化的操作”与“使用对象的操作”分开，让使用者不用知道具体参数就可以实例化出所需要的“产品”类，从而避免了在客户端代码中显式指定，实现了解耦。

### 简单工厂场景
![](https://upload-images.jianshu.io/upload_images/944365-652fab6e6ea33571.png)

### 简单工厂举例
```java
abstract class Product{

	public abstract void show();

}


pbulic class ProductA extends Product{
	@Override 
	public void show() {

	}
}

pbulic class ProductB extends Product{
	@Override 
	public void show() {

	}
}

public class Factory {

	public static Product manufacture(String type) {
		switch(type) {
			case "A"
                return new ProcuctA();
				break;
			case "B"
			    return new ProductB();
				break;
			default;
                return null;
		}
	}
}

public static void main(String[] args) {
	Factory factory = new Factory();
	ProductA product = (ProductA)factory.manufacture("A");
}

```
缺点在于如果增加了产品类型，必须要修改工厂类，违背了开闭原则。

## 工厂方法模式

### 工厂方法定义
工厂方法模式，又称工厂模式、多态工厂模式和虚拟构造器模式，通过定义工厂父类负责定义创建对象的公共接口，而子类则负责生成具体的对象。

将类的实例化（具体产品的创建）延迟到工厂类的子类（具体工厂）中完成，即由子类来决定应该实例化（创建）哪一个类。

### 工厂方法场景
![](https://imgconvert.csdnimg.cn/aHR0cDovL3VwbG9hZC1pbWFnZXMuamlhbnNodS5pby91cGxvYWRfaW1hZ2VzLzk0NDM2NS0yNzc2NDcwMmEzMjgzNGEzLnBuZz9pbWFnZU1vZ3IyL2F1dG8tb3JpZW50L3N0cmlwJTdDaW1hZ2VWaWV3Mi8yL3cvMTI0MA)

### 工厂方法举例
```java
步骤1： 创建抽象工厂类，定义具体工厂的公共接口

abstract class Factory{
    public abstract Product Manufacture();
}

步骤2： 创建抽象产品类 ，定义具体产品的公共接口；

abstract class Product{
    public abstract void Show();
}

步骤3： 创建具体产品类（继承抽象产品类）， 定义生产的具体产品；

//具体产品A类
class  ProductA extends  Product{
    @Override
    public void Show() {
        System.out.println("生产出了产品A");
    }
}

//具体产品B类
class  ProductB extends  Product{

    @Override
    public void Show() {
        System.out.println("生产出了产品B");
    }
}

步骤4：创建具体工厂类（继承抽象工厂类），定义创建对应具体产品实例的方法；

//工厂A类 - 生产A类产品
class  FactoryA extends Factory{
    @Override
    public Product Manufacture() {
        return new ProductA();
    }
}

//工厂B类 - 生产B类产品
class  FactoryB extends Factory{
    @Override
    public Product Manufacture() {
        return new ProductB();
    }
}

步骤5：外界通过调用具体工厂类的方法，从而创建不同具体产品类的实例

//生产工作流程
public class FactoryPattern {
    public static void main(String[] args){
        //客户要产品A
        FactoryA mFactoryA = new FactoryA();
        mFactoryA.Manufacture().Show();

        //客户要产品B
        FactoryB mFactoryB = new FactoryB();
        mFactoryB.Manufacture().Show();
    }
}

结果：

生产出了产品A
生产出了产品B

```