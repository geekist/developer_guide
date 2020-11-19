

# Thread
几乎完全采用了Java中的线程机制。线程是最小的调度单位，在很多情况下为了使APP更加流程地运行，我们不可能将很多事情都放在主线程上执行，这样会造成严重卡顿（ANR），那么这些事情应该交给子线程去做，但对于一个系统而言，创建、销毁、调度线程的过程是需要开销的，所以我们并不能无限量地开启线程，那么对线程的了解就变得尤为重要了。

## Thread的定义和用法
一般实现线程的方法有两种，一种是类继承Thread，一种是实现接口Runnable。这两种方式的优缺点如何呢？我们知道Java是单继承但可以调用多个接口，所以看起来Runnable更加好一些。

```kotlin
class MyThread extend Thread(){

	@Override
	public void run() {
		super.run();
		Log.i(Thread.currentThread().getId());
	}
}
new MyThread().start();
```

```java
class MyThread implements Runnable{
	@Override
	public void run() {
		Log.i("MyThread", Thread.currentThread().getName());
	}
}

MyThreaed myThread = new MyThread();
new Thread(myThread).start();
```

在kotlin中，用lambda表达式对线程的写法进行了优化，甚至提供了一个标准的函数thread。

```kotlin
Thread {
	......
}.start()


thread{
	......
}
```

## Thread/Runnable/Callable
Callable 接口下的方法是 call()，Runnable 接口的方法是 run()；
 
Callable 的任务执行后可返回值，而 Runnable 的任务是不能返回值的；

call() 方法可以抛出异常，run()方法不可以的；

运行 Callable 任务可以拿到一个 Future 对象，表示异步计算的结果。它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。通过 Future 对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果；



