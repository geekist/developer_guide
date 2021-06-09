
# 一、依赖和依赖注入

## 1、依赖注入的定义

在我们的开发实践中，随处可见一个类的定义或者实现需要用到另外一个或多个类。例如：

```java
public class Engine {

    public Engine() {

    }
}

public class Car{
	private Engine engine;  //在Car类中包含了一个Engine类
	
	public Car() {
	}
}
```

```java
public class Teacher{
	public int id；
	public String name;
	
    public Teacher() {

    }
}

public class Student{
	private int teacherId  //在Student类中需要Teacher类的信息
	
	public Student(Teacher teacher) {
		teacherId = teacher.id;
	}
}
```


这种一个类对于另外一个或多个类的引用就是依赖。这样的依赖贯穿于我们每天的coding之中。上面的例子中，我们可以说Teacher类就是Student类的依赖项，Engine类就是Car类的依赖项。或者说，“学校”类依赖于“学生”类，“汽车”类依赖于“引擎”类。

为目标类提供依赖的方式可以分为两种，一种是直接将依赖项的构造方式暴露出来，在目标类中直接调用依赖项的构造方法，比如：
```java
public class Car{
	private Engine engine;
	
	public Car() {
		engine = new Engine(); //直接调用依赖项的构造方法
	}
}
```
另外一种方法是将依赖对象和其构造方式解耦，通过其他的方式传递依赖给目标调用者，即依赖是被“注入”进来的，如：
```java
public class Car{
	private Engine engine;
	
	public Car() {
	}
	
	public void setEngine(Engine engine) {//通过参数传递的方式实现依赖
		this.engine = engine;
	}
}
```
这用依赖实现方式就称为依赖注入。

显然，相比于前者，依赖注入的方式有更明显的优点：

- 构造和使用分离：
将依赖项和目标调用者解耦，当依赖项的构造方式改变时，调用者不需要做任何代码上的改动。

- 便于对依赖项单元测试：
因为实现了依赖类和目标类的解耦，依赖注入更方便做单元测试，我们可以很容易地为上面的类编写单元测试的案例。

-  便于独立/并行开发模块化的代码.
依赖注入相当于给目标调用类提供了一个依赖项的接口，目标类和依赖类可以并行开发自己的模块，不会互相干扰。

## 2、依赖注入的方式

通常依赖注入有以下四种方式

- 1、通过构造方法注入：
```java
public class Car{    
	Engine engine;    
	public void Car(Engine engine) {  //通过带参数的构造器实现依赖注入      
		this.engine = engine;    
	}
}
```
- 2、通过set方法注入
```java
public class Car{    
	Engine engine;    
	public void setEngine(Engine engine) { //通过set方法实现依赖注入        
		this.engine = engine;    
	}
}
```
- 3、通过接口注入：
```java
interface EngineInterface {    
	public void engine(Engine engine);
}

public class Car implements EngineInterface {    
	Engine engine;        

	@override      
	public void engine(Engine engine) {            
		this.engine = engine;        
	}  
}
```

- 4、通过注解的方式注入：
 ```java
 public Car {    
	//需要依赖注入框架的支持，如Dagger2    
	@inject    
	Engine engine;    

	public Car() {} {
	}

}
 ```