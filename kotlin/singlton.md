# Kotlin中的5种单例模式

## 1-饿汉模式

```java

//Java实现
public class SingletonDemo {
	private static SingletonDemo instance=new SingletonDemo();
	private SingletonDemo(){

	}
	public static SingletonDemo getInstance(){
		return instance;
	}
}
//Kotlin实现

object SingletonDemo

```

## 2-懒汉模式
```java
public static Singleton {
	private static Singleton instance;

	private Singleton() {

	}
	
	public static Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton()
		}

		return instance;
	}

//Kotlin实现
class SingletonDemo private constructor() {
	companion object {
		private var instance: SingletonDemo? = null
		get() {
			if (field == null) {
				field = SingletonDemo()
			}
			return field
		}

		fun get(): SingletonDemo{
		//这里不用getInstance作为为方法名，是因为在伴生对象声明时，内部已有getInstance方法，所以只能取其他名字
			return instance!!
		}
	}
}

```

## 3-线程安全的饿汉模式
```java
public static Singleton {
	private static Singlton instance;
	private Singlton() {
	}
    public static synchronized getInstance() {
			if(instance == null) {
				insance = new Singlton();
			}
			return instance;
	}
}

/Kotlin实现
class SingletonDemo private constructor() {
	companion object {
		private var instance: SingletonDemo? = null
		get() {
			if (field == null) {
				field = SingletonDemo()
			}
			return field
		}

		@Synchronized
		fun get(): SingletonDemo{
			return instance!!
		}
	}

}
```

## 4-双重校验锁式

```java
//Java实现
public class SingletonDemo {
private volatile static SingletonDemo instance;
private SingletonDemo(){}
public static SingletonDemo getInstance(){
if(instance==null){
synchronized (SingletonDemo.class){
if(instance==null){
instance=new SingletonDemo();
}
}
}
return instance;
}
}
//kotlin实现
class SingletonDemo private constructor() {
	companion object {
		val instance: SingletonDemo by lazy(mode = LazyThreadSafetyMode.SYNCHRONIZED) {
			SingletonDemo() }
	}
}

```
Lazy是接受一个 lambda 并返回一个 Lazy 实例的函数，返回的实例可以作为实现延迟属性的委托： 第一次调用 get() 会执行已传递给 lazy() 的 lambda 表达式并记录结果， 后续调用 get() 只是返回记录的结果。

## 5-静态内部类模式
```java
//Java实现
public class SingletonDemo {
	private static class SingletonHolder{
		private static SingletonDemo instance=new SingletonDemo();
	}

	private SingletonDemo(){
		System.out.println("Singleton has loaded");
	}

	public static SingletonDemo getInstance(){
		return SingletonHolder.instance;
	}
}
//kotlin实现
class SingletonDemo private constructor() {
	companion object {
		val instance = SingletonHolder.holder
	}

	private object SingletonHolder {
		val holder= SingletonDemo()
	}
}
```