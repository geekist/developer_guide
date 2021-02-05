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

饿汉模式在类被初始化时就已经在内存中创建了对象，以空间换时间，故不存在线程安全问题。


## 2-懒汉模式
```java
public class Singleton {
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

懒汉模式在方法被调用后才创建对象，以时间换空间，在多线程环境下存在风险。



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

    private SingletonDemo(){

    }

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

Lazy是接受一个 lambda 并返回一个 Lazy 实例的函数，返回的实例可以作为实现延迟属性的委托： 第一次调用 get() 会执行已传递给 lazy() 的 lambda 表达式并记录结果， 后续调用 get() 只是返回记录的结果。

```
DCL（double check lock）模式的优点就是，只有在对象需要被使用时才创建，第一次判断 INSTANCE == null为了避免非必要加锁，当第一次加载时才对实例进行加锁再实例化。这样既可以节约内存空间，又可以保证线程安全。但是，由于jvm存在乱序执行功能，DCL也会出现线程不安全的情况。

不过在JDK1.5之后，官方也发现了这个问题，故而具体化了volatile，即在JDK1.6及以后，只要定义为private volatile static SingleTon  INSTANCE = null;就可解决DCL失效问题。volatile确保INSTANCE每次均在主内存中读取，这样虽然会牺牲一点效率，但也无伤大雅。


## 5-静态内部类模式

```java

//Java实现

public class SingletonDemo {

    private SingletonDemo(){

	}

    public static SingletonDemo getInstance(){
        return SingletonHolder.instance;
	}

	private static class SingletonHolder{
		private static SingletonDemo instance=new SingletonDemo();
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

静态内部类特点
1）外部类装载的时候，静态内部类不会装载

2）静态类所在的外部类使用内部类时，静态内部类会装载

3）静态内部类在装载时是线程安全的，只会装载一次

使用静态内部类优缺点分析

1）这种方式采用了类装载的机制来保证初始化实例时只有一个线程

2）静态内部类方式在Singleton类被装载时并不会立即实例化，而是在需要实例化时，调用getInstance方法，才会装载SingletonInstance类，从而完成Singleton的实例化

3）类的静态属性只会在第一次加载类的时候初始化，所以在这里，JVM帮助我们保证了线程的安全性，在类进行初始化时，别的线程是无法进入的

4）优点：避免了线程不安全，利用静态内部类特点实现延迟加载，效率高