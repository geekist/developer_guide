
# 单例模式

## 饿汉模式

```java

public class Singleton {

	private static Singleton instance = new Singleton();

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

public class SingleTon{
  private SingleTon(){}
 
  private static class SingleTonHoler{
     private static SingleTon INSTANCE = new SingleTon();
 }
 
  public static SingleTon getInstance(){
    return SingleTonHoler.INSTANCE;
  }
}

```

用静态内部列实现单例模式，既能保证延迟加载，又能保证线程安全，只创建一个实例对象。

* 1、java类加载的生命周期：


类从被加载到虚拟机内存中开始，到卸载出内存为止，整个生命周期包括：加载、验证、准备、解析、初始化、使用、卸载 7 个阶段。

![](https://upload-images.jianshu.io/upload_images/2756772-76402efe48ae130e.jpg)

* 加载：

在这个阶段虚拟机需要完成三件事：
1、通过一个类的全限定名来获取定义此类的二进制字节流。
2、将这个字节流代表的静态存储结构转化为方法区的运行时数据结构。
3、在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区这个类的各种数据的访问入口。

* 验证：

这阶段的目的是为了确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机自身的安全。

* 准备：

此阶段是正式为类变量分配内存并设置类变量初始值的阶段，这些变量使用的内存都将在方法区中分配。如，int 类型的默认值为 0 ，布尔类型的默认值为 false，非基本类型的初始值为 null 。

* 解析：

此阶段是虚拟机将常量池的符号引用替换为直接引用的过程。


* 初始化：

初始化阶段即 执行类的 clinit() 方法的阶段（clinit = class + initialize，意为类的初始化），包括为类的静态变量赋初始值和执行静态代码块中的内容，执行的先后顺序取决于在源文件中出现的顺序。

对于静态内部类的加载，只是先存放在方法区，当调用到该内部类时，才开始加载，实现了延迟加载。

同样，虚拟机会保证一个类的初始化方法在多线程环境中是安全的。

