
# Android 线程和线程池


## 引言
在Android中，几乎完全采用了Java中的线程机制。线程是最小的调度单位，在很多情况下为了使APP更加流程地运行，我们不可能将很多事情都放在主线程上执行，这样会造成严重卡顿（ANR），那么这些事情应该交给子线程去做，但对于一个系统而言，创建、销毁、调度线程的过程是需要开销的，所以我们并不能无限量地开启线程，那么对线程的了解就变得尤为重要了。

## Thread/Runnable/Callable
一般实现线程的方法有两种，一种是类继承Thread，一种是实现接口Runnable。这两种方式的优缺点如何呢？我们知道Java是单继承但可以调用多个接口，所以看起来Runnable更加好一些。

继承Thread
```java
class MyThread extend Thread(){

@Override

public void run() {

super.run();

Log.i(Thread.currentThread().getId());

}

}

new MyThread().start();

```


实现Runnable接口

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

当我们调用Thread时，会有两种方式：

Thread myThread = new Thread();

myThread.run();

myThread.start();

我们应该知道，run()方法只是调用了Thread实例的run()方法而已，它仍然运行在主线程上，而start()方法会开辟一个新的线程，在新的线程上调用run()方法，此时它运行在新的线程上。

Runnable只是一个接口，所以单看这个接口它和线程毫无瓜葛，可能一部分人会以为Runnable实现了线程，这种理解是不对的。Thread调用了Runnable接口中的方法用来在线程中执行任务。


## Runable和Callable
```java
public interface Runnable {

public void run();

}



public interface Callable<V> {

V call() throws Exception;

}
```


Runnable 和 Callable 都代表那些要在不同的线程中执行的任务。Runnable 从 JDK1.0 开始就有了，Callable 是在 JDK1.5 增加的。它们的主要区别是 Callable 的 call() 方法可以返回值和抛出异常，而 Runnable 的 run() 方法没有这些功能。Callable 可以返回装载有计算结果的 Future 对象。

我们通过对比两个接口得到这样的结论：

Callable 接口下的方法是 call()，Runnable 接口的方法是 run()；

Callable 的任务执行后可返回值，而 Runnable 的任务是不能返回值的；

call() 方法可以抛出异常，run()方法不可以的；

运行 Callable 任务可以拿到一个 Future 对象，表示异步计算的结果。它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。通过 Future 对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果；

然而…Thread类只支持Runnable接口，由此引入FutureTask的概念。

##FutureTask

FutureTask
FutureTask 实现了 Runnable 和 Future，所以兼顾两者优点，既可以在 Thread 中使用，又可以在 ExecutorService 中使用。
```java

public interface Future<V> {



boolean cancel(boolean mayInterruptIfRunning);



boolean isCancelled();



boolean isDone();



V get() throws InterruptedException, ExecutionException;



V get(long timeout, TimeUnit unit)

throws InterruptedException, ExecutionException, TimeoutException;

}
```

使用 FutureTask 的好处是 FutureTask 是为了弥补 Thread 的不足而设计的，它可以让程序员准确地知道线程什么时候执行完成并获得到线程执行完成后返回的结果。FutureTask 是一种可以取消的异步的计算任务，它的计算是通过 Callable 实现的，它等价于可以携带结果的 Runnable，并且有三个状态：等待、运行和完成。完成包括所有计算以任意的方式结束，包括正常结束、取消和异常。


## AsyncTask、HandlerThread、IntentService

除了以上这些，在Android中充当线程的角色还有AsyncTask、HandlerThread、IntentService。它们本质上都是由Handler+Thread来构成的，不过不同的设计让它们可以在不同的场合发挥更好的作用。我们来简单地说一下它们各自的特点：

AsyncTask，它封装了线程池和Handler，主要为我们在子线程中更新UI提供便利。

HandlerThread，它是个具有消息队列的线程，可以方便我们在子线程中处理不同的事务。

IntentService，我们可以将它看做为HandlerThread的升级版，它是服务，优先级更高。

下面我来通过源码，用法等来认清这些线程。

### AsyncTask
AsyncTask是一个轻量级的异步任务类，它可以在线程池中执行后台任务，然后把执行的进度和结果传递给主线程并且在主线程中更新UI。

#### 声明

AsyncTask是一个抽象泛型类，声明：public abstract class AsyncTask<Params, Progress, Result>;

- 参数1，Params，异步任务的入参；

- 参数2，Progress，执行任务的进度；

- 参数3，Result，后台任务执行的结果；

- 并且提供了4个核心方法。

* 方法1， onPreExecute()，在主线程中执行，任务开启前的准备工作；

* 方法2，doInbackground(Params…params)，开启子线程执行后台任务；

* 方法3，onProgressUpdate(Progress values)，在主线程中执行，更新UI进度；

* 方法4，onPostExecute(Result result)，在主线程中执行，异步任务执行完成后执行，它的参数是doInbackground()的返回值。

#### 用法

来个一般的写法吧：
```java

private class MyAsyncTask extends AsyncTask{
	private String mName;

	public MyAsyncTask(String name){
		mName = name;
	}


	@TargetApi(Build.VERSION_CODES.N)
	@Override
	protected Object doInBackground(Object[] params) {
		try {
			Thread.sleep(3000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		Log.i(mName, dateFormat.format(new Date(System.currentTimeMillis())));
		return null;
	}
}
```
我们可以通过这样的调用来验证其是串行还是并发的。
```java
(new MyAsyncTask("MyAsyncTask01")).execute();

(new MyAsyncTask("MyAsyncTask02")).execute();

(new MyAsyncTask("MyAsyncTask03")).execute();
```

可以看到这三个异步任务的执行时间是

I/MyAsyncTask01: 2017-05-26 20:35:29

I/MyAsyncTask02: 2017-05-26 20:35:32

I/MyAsyncTask03: 2017-05-26 20:35:35

由此可知它的执行是串行的，如果需要并发执行，调用executeOnExecutor()即可。
```java
(new MyAsyncTask("MyAsyncTask01")).executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, "");

(new MyAsyncTask("MyAsyncTask02")).executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, "");

(new MyAsyncTask("MyAsyncTask03")).executeOnExecutor(AsyncTask.THREAD_POOL_EXECUTOR, "");
```
看时间是这样的：

I/MyAsyncTask02: 2017-05-26 20:38:52

I/MyAsyncTask03: 2017-05-26 20:38:52

I/MyAsyncTask01: 2017-05-26 20:38:52

### HandlerThread
HandlerThread继承了Thread，我们都知道如果需要在线程中创建一个可接收消息的HandlerHandlerThread实际上是一个允许Handler的特殊线程。

```java
@Override

public void run() {

mTid = Process.myTid();

Looper.prepare();

synchronized (this) {

mLooper = Looper.myLooper();

notifyAll();

}

Process.setThreadPriority(mPriority);

onLooperPrepared();

Looper.loop();

mTid = -1;

}
```

普通线程在run()方法中执行耗时操作，而HandlerThread在run()方法创建了一个消息队列不停地轮询消息，我们可以通过Handler发送消息来告诉线程该执行什么操作。

它在Android中是个很有用的类，它常见的使用场景实在IntentService中。当我们不再需要HandlerThread时，我们通过调用quit/Safely方法来结束线程的轮询并结束该线程。

### IntentService
1. 概述

IntentService是一个继承Service的抽象类，所以我们必须实现它的子类再去使用。

在说到HandlerThread时我们提到，HandlerThread的使用场景是在IntentService上，我们可以这样来理解IntentService，它是一个实现了HandlerThread的Service。

那么为什么要这样设计呢？这样设计的好处是Service的优先级比较高，我们可以利用这个特性来保证后台服务的优先正常执行，甚至我们还可以为Service开辟一个新的进程。


## 初识线程池
我们在上面的篇幅中，讲到了线程的概念以及一些扩展线程。那么我们考虑一个问题，我如果需要同时做很多事情，是不是给每一个事件都开启一个线程呢？那如果我的事件无限多呢？频繁地创建/销毁线程，CPU该吃不消了吧。所以，这时候线程池的概念就来了。我们举个例子来阐述一下线程池大致工作原理。

比如，有个老板戚总开了个饭店，每到中午就有很多人点外卖，一开始戚总招了10个人送外卖，然而由于午饭高峰期可能同时需要派送50份外卖，那如何保证高效地运行呢？

戚总想着那再招40个员工送？我去，那我这店岂不是要赔死，人员工资这么高，并且大部分时候也只需要同时派送几份外卖而已，招这么多人干瞪眼啊，是啊。但我还得保证高峰期送餐效率，咋办呢？

经过一番思想斗争，戚总想通了，我也不可能做到完美，尽量高效就行了，那正常时间一般只需要同时送四五家外卖，那我就招5个员工作为正式员工（核心线程），再招若干兼职（非核心线程）在用餐高峰时缓解一下送餐压力即可。

那么，人员分配方案出来了，当正式员工（核心线程）空闲时有单进来理所应当让他们派送，如果正式员工忙不过了，就让兼职人员（非核心线程）送，按单提成唄。

好吧，啰嗦这么多，这就是线程池的概念原理吧。

我们来总结一下优点吧。

重用线程池中的线程，避免频繁地创建和销毁线程带来的性能消耗；

有效控制线程的最大并发数量，防止线程过大导致抢占资源造成系统阻塞；

可以对线程进行一定地管理。

### ThreadPoolExecutor
ExecutorService是最初的线程池接口，ThreadPoolExecutor类是对线程池的具体实现，它通过构造方法来配置线程池的参数，我们来分析一下它常用的构造函数吧。

```java
public ThreadPoolExecutor(
	int corePoolSize,

	int maximumPoolSize,

	long keepAliveTime,

	TimeUnit unit,

	BlockingQueue<Runnable> workQueue) 

{
 this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,

Executors.defaultThreadFactory(), defaultHandler);

}
```

参数解释：

corePoolSize，线程池中核心线程的数量，默认情况下，即使核心线程没有任务在执行它也存在的，我们固定一定数量的核心线程且它一直存活这样就避免了一般情况下CPU创建和销毁线程带来的开销。我们如果将ThreadPoolExecutor的allowCoreThreadTimeOut属性设置为true，那么闲置的核心线程就会有超时策略，这个时间由keepAliveTime来设定，即keepAliveTime时间内如果核心线程没有回应则该线程就会被终止。allowCoreThreadTimeOut默认为false，核心线程没有超时时间。

maximumPoolSize，线程池中的最大线程数，当任务数量超过最大线程数时其它任务可能就会被阻塞。最大线程数=核心线程+非核心线程。非核心线程只有当核心线程不够用且线程池有空余时才会被创建，执行完任务后非核心线程会被销毁。

keepAliveTime，非核心线程的超时时长，当执行时间超过这个时间时，非核心线程就会被回收。当allowCoreThreadTimeOut设置为true时，此属性也作用在核心线程上。

unit，枚举时间单位，TimeUnit。

workQueue，线程池中的任务队列，我们提交给线程池的runnable会被存储在这个对象上。

### 线程池的分配遵循这样的规则：

当线程池中的核心线程数量未达到最大线程数时，启动一个核心线程去执行任务；

如果线程池中的核心线程数量达到最大线程数时，那么任务会被插入到任务队列中排队等待执行；

如果在上一步骤中任务队列已满但是线程池中线程数量未达到限定线程总数，那么启动一个非核心线程来处理任务；

如果上一步骤中线程数量达到了限定线程总量，那么线程池则拒绝执行该任务，且ThreadPoolExecutor会调用RejectedtionHandler的rejectedExecution方法来通知调用者。

### 线程池的分类
我们来介绍一下不同特性的线程池，它们都直接或者间接通过ThreadPoolExecutor来实现自己的功能。它们分别是：

* FixedThreadPool

* CachedThreadPool

* ScheduledThreadPool

* SingleThreadExecutor

#### 1. FixedThreadPool

通过Executors的newFixedThreadPool()方法创建，它是个线程数量固定的线程池，该线程池的线程全部为核心线程，它们没有超时机制且排队任务队列无限制，因为全都是核心线程，所以响应较快，且不用担心线程会被回收。
```java
public static ExecutorService newFixedThreadPool(int nThreads){

	return new ThreadPoolExecutor(
		nThreads, 
		nThreads, 
		0L, 
		TimeUnit.MILLISECONDS,
		new LinkedBlockingQueue<Runnable>()
	);
}

ExecutorService mExecutor = Executors.newFixedThreadPool(5);
```

参数nThreads，就是我们固定的核心线程数量。

#### 2. CachedThreadPool

通过Executors的newCachedThreadPool()方法来创建，它是一个数量无限多的线程池，它所有的线程都是非核心线程，当有新任务来时如果没有空闲的线程则直接创建新的线程不会去排队而直接执行，并且超时时间都是60s，所以此线程池适合执行大量耗时小的任务。由于设置了超时时间为60s，所以当线程空闲一定时间时就会被系统回收，所以理论上该线程池不会有占用系统资源的无用线程。
```java
public static ExecutorService new CachedThreadPool(){

	return new ThreadPoolExecutor(
		0, 
		Integer.MAX_VALUE, 
		60L, 
		TimeUnit.SECONDS,
		new SynchronousQueue<Runnable>()
	);
}

```

#### 3. ScheduledThreadPool

通过Executors的newScheduledThreadPool()方法来创建，ScheduledThreadPool线程池像是上两种的合体，它有数量固定的核心线程，且有数量无限多的非核心线程，但是它的非核心线程超时时间是0s，所以非核心线程一旦空闲立马就会被回收。这类线程池适合用于执行定时任务和固定周期的重复任务。
```java
public static ScheduledThreadPool newScheduledThreadPool(int corePoolSize){

	return new ScheduledThreadPoolExecutor(corePoolSize);
}

public ScheduledThreadPoolExecutor(int corePoolSize){

	super(corePoolSize, 
			Integer.MAX_VALUE, 
			0, 
			NANOSECONDS,
			new DelayedWorkQueue());
}
```

参数corePoolSize是核心线程数量。

#### 4. SingleThreadExecutor

通过Executors的newSingleThreadExecutor()方法来创建，它内部只有一个核心线程，它确保所有任务进来都要排队按顺序执行。它的意义在于，统一所有的外界任务到同一线程中，让调用者可以忽略线程同步问题。
```java
public static ExecutorService newSingleThreadExecutor(){

	return new FinalizableDelegatedExecutorService(
		new ThreadPoolExecutor(
		1, 
		1, 
		0L, 
		TimeUnit.MILLISECONDS,
		new LinkedBlockingQueue<Runnable>()
		)
	);
}
```

### 线程池一般用法
* shutDown()，关闭线程池，需要执行完已提交的任务；

* shutDownNow()，关闭线程池，并尝试结束已提交的任务；

* allowCoreThreadTimeOut(boolen)，允许核心线程闲置超时回收；

* execute()，提交任务无返回值；

* submit()，提交任务有返回值；

### 自定义线程池
```java
ExecutorService mExecutor = Executors.newFixedThreadPool(5);


//execute()方法
//接收一个Runnable对象作为参数，异步执行。

Runnable myRunnable = new Runnable() {

	@Override
	public void run() {
		Log.i("myRunnable", "run");
	}
};

mExecutor.execute(myRunnable);
```

