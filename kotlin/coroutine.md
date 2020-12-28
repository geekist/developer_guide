# Kotlin协程

* 协程的依赖项：


所谓协程，就是运行在一个协程域中的轻量级的线程。 

## 一、协程域（CoroutineScope）
```java
public interface CoroutineScope {
    public val coroutineContext: CoroutineContext
}
```
### 1-协程域的创建： **协程域构建器**

* GlaobalScope是协程域的一个实现

```java
public object GlobalScope : CoroutineScope {
    override val coroutineContext: CoroutineContext
        get() = EmptyCoroutineContext
}

val job = GlobalScope.launch { // 启动⼀个新协程并保持对这个作业的引⽤
delay(1000L)
println("World!")
}
println("Hello,")
job.join() // 等待直到⼦协程执⾏结束
```
* MainScope是一个顶层函数，该函数的返回值是一个协程域

```java
@Suppress("FunctionName")
public fun MainScope(): CoroutineScope = ContextScope(SupervisorJob() + Dispatchers.Main)
```

*  runBlocking是一个顶层函数，第一个参数是协程上下文，默认是空，第二个参数是一个协程类的挂起函数
其在当前线程中启动，会阻塞当前线程。
```java
@Throws(InterruptedException::class)
public fun <T> runBlocking(context: CoroutineContext = EmptyCoroutineContext, block: suspend CoroutineScope.() -> T): T {
    val currentThread = Thread.currentThread()
    val contextInterceptor = context[ContinuationInterceptor]
    val eventLoop: EventLoop?
    val newContext: CoroutineContext
    if (contextInterceptor == null) {
        // create or use private event loop if no dispatcher is specified
        eventLoop = ThreadLocalEventLoop.eventLoop
        newContext = GlobalScope.newCoroutineContext(context + eventLoop)
    } else {
        // See if context's interceptor is an event loop that we shall use (to support TestContext)
        // or take an existing thread-local event loop if present to avoid blocking it (but don't create one)
        eventLoop = (contextInterceptor as? EventLoop)?.takeIf { it.shouldBeProcessedFromContext() }
            ?: ThreadLocalEventLoop.currentOrNull()
        newContext = GlobalScope.newCoroutineContext(context)
    }
    val coroutine = BlockingCoroutine<T>(newContext, currentThread, eventLoop)
    coroutine.start(CoroutineStart.DEFAULT, coroutine, block)
    return coroutine.joinBlocking()
}

```

* coroutineScope函数构造的协程域，参数是一个协程域的挂起函数
构建器声明⾃⼰的作⽤域。它会创建⼀个协程作⽤域并且在所有已启动⼦协程执⾏完毕之前不会结束
```java
public suspend fun <R> coroutineScope(block: suspend CoroutineScope.() -> R): R =
    suspendCoroutineUninterceptedOrReturn { uCont ->
        val coroutine = ScopeCoroutine(uCont.context, uCont)
        coroutine.startUndispatchedOrReturn(coroutine, block)
    }


fun main() = runBlocking { // 创建一个协程域，阻塞当前线程
launch {
	delay(200L)
	println("Task from runBlocking")
}

coroutineScope { // 创建⼀个协程作⽤域，阻塞当前协程
	launch {
		delay(500L)
		println("Task from nested launch")
	}
	delay(100L)
	println("Task from coroutine scope") // 这⼀⾏会在内嵌 launch 之前输出
}

println("Coroutine scope is over") // 这⼀⾏在内嵌 launch 执⾏完毕后才输出
}
```

### 2-CoroutineContext协程上下文
协程总是运⾏在⼀些以 CoroutineContext 类型为代表的上下⽂中，它们被定义在了 Kotlin 的标准库⾥。

协程上下⽂包含⼀个 协程调度器（ 参⻅ CoroutineDispatcher）它确定了相关的协程在哪个线程或哪些线程上执⾏。

协程调度器可以将协程限制在⼀个特定的线程执⾏，或将它分派到⼀个线程池，亦或是让它不受限地运⾏。
所有的协程构建器诸如 launch 和 async 接收⼀个可选的 CoroutineContext 参数，它可以被⽤来显式的为⼀个新协
程或其它上下⽂元素指定⼀个调度器。

*  Dispatchers.Default 代表的默认调度器。 默认调度器使⽤共享的后台线程池。 
  
*  launch(Dispatchers.Default) { …… } 与 GlobalScope.launch { …… } 使⽤相同的调度器。

* Dispatchers.Unconfined 协程调度器在调⽤它的线程启动了⼀个协程，但它仅仅只是运⾏到第⼀个挂起点。挂起后，它恢复线程中的协程，⽽这完全由被调⽤的挂起函数来决定。⾮受限的调度器⾮常适⽤于执⾏不消耗 CPU 时间的任务，以及不更新局限于特定线程的任何共享数据（如UI）的协程。


```java
fun main() = runBlocking<Unit> {
    launch { // context of the parent, main runBlocking coroutine
        println("main runBlocking      : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Unconfined) { // not confined -- will work with main thread
        println("Unconfined            : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(Dispatchers.Default) { // will get dispatched to DefaultDispatcher 
        println("Default               : I'm working in thread ${Thread.currentThread().name}")
    }
    launch(newSingleThreadContext("MyOwnThread")) { // will get its own new thread
        println("newSingleThreadContext: I'm working in thread ${Thread.currentThread().name}")
    }
}


```
## 2-协程

### 2.1 创建和启动一个协程：
* 在协程域中使用协程域扩展函数launch函数创建和启动一个协程,第一个参数是协程域上下文，第二个参数是启动模式，第三个参数是一个挂起函数。返回值是一个Job类型。即协程类型。
* 
```java
public fun CoroutineScope.launch(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> Unit
): Job {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyStandaloneCoroutine(newContext, block) else
        StandaloneCoroutine(newContext, active = true)
    coroutine.start(start, coroutine, block)
    return coroutine
}
```

* async是一个协程域扩展函数，用于创建一个协程，返回一个Deferred类实例，该实例有一个await函数，可以在协程结束后返回函数值。
 
async 就类似于 launch。它启动了⼀个单独的协程，这是⼀个轻量级的线程并与其它所有的协程⼀起并发的⼯作。不同之处在于 launch 返回⼀个 Job 并且不附带任何结果值，⽽ async 返回⼀个 Deferred ⸺ ⼀个轻量级的⾮阻塞 future， 这代表了⼀个将会在稍后提供结果的 promise。你可以使⽤ .await() 在⼀个延期的值上得到它的最终结果， 但是 Deferred 也是⼀个 Job ，所以如果需要的话，你可以取消它。 

```java
public fun <T> CoroutineScope.async(
    context: CoroutineContext = EmptyCoroutineContext,
    start: CoroutineStart = CoroutineStart.DEFAULT,
    block: suspend CoroutineScope.() -> T
): Deferred<T> {
    val newContext = newCoroutineContext(context)
    val coroutine = if (start.isLazy)
        LazyDeferredCoroutine(newContext, block) else
        DeferredCoroutine<T>(newContext, active = true)
    coroutine.start(start, coroutine, block)
    return coroutine
}

```

```java
val time = measureTimeMillis {
val one = async { doSomethingUsefulOne() }
val two = async { doSomethingUsefulTwo() }
println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```
**懒启动的async扩展函数**
可以通过将 start 参数设置为 CoroutineStart.LAZY .变为惰性的。 在这个模式下，只有结果通过await 获取的时候协程才会启动，或者在 Job 的 start 函数调用的时候
```java
val time = measureTimeMillis {
val one = async(start = CoroutineStart.LAZY) { doSomethingUsefulOne() }
val two = async(start = CoroutineStart.LAZY) { doSomethingUsefulTwo() }
// 执⾏⼀些计算
one.start() // 启动第⼀个
two.start() // 启动第⼆个
println("The answer is ${one.await() + two.await()}")
}
println("Completed in $time ms")
```
**async风格的函数**
```java
// somethingUsefulOneAsync 函数的返回值类型是 Deferred<Int>
fun somethingUsefulOneAsync() = GlobalScope.async {
doSomethingUsefulOne()
}
// somethingUsefulTwoAsync 函数的返回值类型是 Deferred<Int>
fun somethingUsefulTwoAsync() = GlobalScope.async {
doSomethingUsefulTwo()
}

```
### 2.2 取消一个协程
用job.cancel取消一个协程。
我们有两种⽅法来使执⾏计算的代码可以被取消。第⼀种⽅法是定期调⽤挂起函数来检查取消。对于这种⽬的 yield
是⼀个好的选择。 另⼀种⽅法是显式的检查取消状态isActive。

* 使用try finally 
 
在finally中可以执行释放资源的操作，也可以执行当协程被取消是的善后工作，用withContext函数

### 2.3 等待协程结束
用job.join等待协程结束。

### 2.4 cancelAndJoin
合并取消和等待

```java
val job = launch {
repeat(1000) { i ->
println("job: I'm sleeping $i ...")
delay(500L)
}
}
delay(1300L) // 延迟⼀段时间
println("main: I'm tired of waiting!")
job.cancel() // 取消该作业
job.join() // 等待作业执⾏结束
println("main: Now I can quit.")


job: I'm sleeping 0 ...
job: I'm sleeping 1 ...
job: I'm sleeping 2 ...
main: I'm tired of waiting!
main: Now I can quit.
```
### 2.5 协程源码---Job  defred
Job是start一个协程的返回值，可以使用一些方法，如cancel，join等
```java
public interface Job : CoroutineContext.Element {
   
    public companion object Key : CoroutineContext.Key<Job> {
        init {
            CoroutineExceptionHandler
        }
    }

    public val isActive: Boolean
    public val isCompleted: Boolean
    public val isCancelled: Boolean

    @InternalCoroutinesApi
    public fun getCancellationException(): CancellationException

    public fun start(): Boolean

    public fun cancel(cause: CancellationException? = null)
    public fun cancel(): Unit = cancel(null)
    public fun cancel(cause: Throwable? = null): Boolean

    public suspend fun join()

```

```java
public interface Deferred<out T> : Job {
    public suspend fun await(): T
```



## 3-在协程作用域内执行的函数--挂起函数
suspend 修饰符的函数。这能在协程作用域使用。在协程内部可以像普通函数⼀样使⽤挂起函数不过其额外特性是，同样可以使⽤其他挂起函数（如本例中的 delay ）来挂起协程的执⾏。

### delay是一个挂起函数，只能作用于协程域内，将当前协程挂起指定的时间
```java
public suspend fun delay(timeMillis: Long) {
    if (timeMillis <= 0) return // don't delay
    return suspendCancellableCoroutine sc@ { cont: CancellableContinuation<Unit> ->
        cont.context.delay.scheduleResumeAfterDelay(timeMillis, cont)
    }
}
```

### withContext是一个挂起函数，只能用于协程域内部，作用是执行一段不能取消的代码，通常用于try、catch、finally的finally块中，用于执行收尾工作。
```java
public suspend fun <T> withContext(
    context: CoroutineContext,
    block: suspend CoroutineScope.() -> T
): T

```

### withTimeout或者withTimeoutOrNull是挂起函数，只能用于协程类的内部，当执行到超时设定时会取消结束，并抛出一个异常。（后者不抛出异常）
```java
public suspend fun <T> withTimeout(timeMillis: Long, block: suspend CoroutineScope.() -> T): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
    if (timeMillis <= 0L) throw TimeoutCancellationException("Timed out immediately")
    return suspendCoroutineUninterceptedOrReturn { uCont ->
        setupTimeout(TimeoutCoroutine(timeMillis, uCont), block)
    }
}
```

## 6-流

## 7-通道