# Kotlin协程

## 1-CoroutineScope
本质上，协程是轻量级的线程。 它们在某些 CoroutineScope 上下⽂中与 launch 协程构建器 ⼀起启动。

### 协程域
```java
public interface CoroutineScope {
    public val coroutineContext: CoroutineContext
}
```
### GlaobalScope是协程域的一个实现
public object GlobalScope : CoroutineScope {
    
    override val coroutineContext: CoroutineContext
        get() = EmptyCoroutineContext
}

### MainScope函数返回一个协程域
```java
@Suppress("FunctionName")
public fun MainScope(): CoroutineScope = ContextScope(SupervisorJob() + Dispatchers.Main)
```



## 2-CoroutineContext
协程总是运⾏在⼀些以 CoroutineContext 类型为代表的上下⽂中，它们被定义在了 Kotlin 的标准库⾥。
协程上下⽂是各种不同元素的集合。其中主元素是协程中的 Job， 我们在前⾯的⽂档中⻅过它以及它的调度器，
### 协程上下文

### 协程调度模式

当调⽤ launch { …… } 时不传参数，它从启动了它的 CoroutineScope 中承袭了上下⽂（以及调度器）。在这个案例中，它从 main 线程中的 runBlocking 主协程承袭了上下⽂。

*  Dispatchers.Default 代表的默认调度器。 默认调度器使⽤共享的后台线程池。 所以 launch(Dispatchers.Default) { …… } 与 GlobalScope.launch { …… } 使⽤相同的调度器。

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

### 协程---Job  defred
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


## 3-协程构造器


### runBlocking是一个全局函数，接受两个参数：协程域上下文和一个协程域内的挂起函数，返回一个泛型类（挂起函数的返回类型）
```java
@Throws(InterruptedException::class)
public fun <T> runBlocking(context: CoroutineContext = EmptyCoroutineContext, block: suspend CoroutineScope.() -> T): T {
    contract {
        callsInPlace(block, InvocationKind.EXACTLY_ONCE)
    }
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

### launch是一个协程域扩展函数，用于创建一个协程，并返回一个job类实例
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
### async是一个协程域扩展函数，用于创建一个协程，返回一个Deferred类实例，该实例有一个await函数，可以在协程结束后返回函数值。
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


可选的，async 可以通过将 start 参数设置为 CoroutineStart.LAZY ⽽变为惰性的。 在这个模式下，只有结果通过
await 获取的时候协程才会启动，或者在 Job 的 start 函数调⽤的时候




## 4-作用域构造器函数
除了由不同的构建器提供协程作⽤域之外，还可以使⽤ coroutineScope 构建器声明⾃⼰的作⽤域。它会创建⼀个协程作⽤域并且在所有已启动⼦协程执⾏完毕之前不会结束。
runBlocking 与 coroutineScope 可能看起来很类似，因为它们都会等待其协程体以及所有⼦协程结束。 主要区别在于，runBlocking ⽅法会阻塞当前线程来等待， ⽽ coroutineScope 只是挂起，会释放底层线程⽤于其他⽤途。 由于存在这点差异，runBlocking 是常规函数，⽽ coroutineScope 是挂起函数。
```java

import kotlinx.coroutines.*
fun main() = runBlocking { // this: CoroutineScope
launch {
delay(200L)
println("Task from runBlocking")
}
coroutineScope { // 创建⼀个协程作⽤域
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

## 5-挂起函数
suspend 修饰符的函数。这能在协程内使用。在协程内部可以像普通函数⼀样使⽤挂起函数不过其额外特性是，同样可以使⽤其他挂起函数（如本例中的 delay ）来挂起协程的执⾏。

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