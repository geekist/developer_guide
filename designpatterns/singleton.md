
# 单例模式

## 饿汉模式

```java

public class Singleton {

	private Singleton instance = new Singleton();

	private singleton() {
	}

	public static Singleton getInstance() {
		return instance;
	}
}

```

## 懒汉模式

```java
public class Singleton {

	private static Singleton instance;

	private singleton() {
	}

	public static Singleton getInstance() {
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

## 线程安全的懒汉模式
```java
public class Singleton {

	private static Singleton instance;

	private singleton() {
	}


	public stataic synchronized Singleton getInstance(){
		if(instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}

```

## 双重检查的懒汉模式
```java

public class Singleton{

	private static volatile Singleton instance;

	private Singleton() {

	}

	public static Singleton getInstance() {
		if(instance != null) {
			return instance;
		}
		
		synchronized(Singleton.class) {
			if(instance == null) {
				instance = new Singleton();
			}
		}

		return instance;

	}

}

```

## 静态内部类
```java
public class Singleton {
	private Singleton() {
	}

	public static Singleton getInstance() {
		return Singleton.instance;
	}

	private static class SingletonHolder {
		private static final Singleton instance = new Singleton();
	}
}
```
