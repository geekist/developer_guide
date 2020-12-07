# [RxJava3.0 介绍](https://juejin.cn/post/6844903885245513741)

# [RxJava2 操作符总结(Kotlin)版][rxjava+kotlin]
[rxjava+kotlin]:https://github.com/geekist/developer_guide/blob/main/android/libraries/RxJava+Kotlin.md

# RxJava2
RxJava到底是什么？异步！好在哪里？简洁。因为用了链式结构，把什么复杂逻辑都能穿成一条线的简洁。

## 一、RxJava介绍和原理简析

### 1、概念

#### 1-1概念：扩展的观察者模式
 
RxJava 的异步实现，是通过一种扩展的观察者模式来实现的。

* 观察者模式

观察者模式面向的需求是：A 对象（观察者）对 B 对象（被观察者）的某种变化高度敏感，需要在 B 变化的一瞬间做出反应。

程序的观察者模式，观察者不需要时刻盯着被观察者（例如 A 不需要每过 2ms 就检查一次 B 的状态），而是采用注册(Register)或者称为订阅(Subscribe)的方式，告诉被观察者：我需要你的某某状态，你要在它变化的时候通知我。 

Android 开发中一个比较典型的例子是点击监听器 OnClickListener 。

对设置 OnClickListener 来说， View 是被观察者， OnClickListener 是观察者，二者通过 setOnClickListener() 方法达成订阅关系。订阅之后用户点击按钮的瞬间，Android Framework 就会将点击事件发送给已经注册的 OnClickListener 。采取这样被动的观察方式，既省去了反复检索状态的资源消耗，也能够得到最高的反馈速度。

#### 1-2 RxJava 的观察者模式

RxJava 有四个基本概念：Observable (可观察者，即被观察者)、 Observer (观察者)、 subscribe (订阅)、 事件。


* Observable

* Observer
与传统观察者模式不同， Observer的事件回调方法除了普通事件 onNext() （相当于 onClick() / onEvent()）之外，还定义了两个特殊的事件：onCompleted() 和 onError()。

onCompleted(): 事件队列完结。RxJava 不仅把每个事件单独处理，还会把它们看做一个队列。RxJava 规定，当不会再有新的 onNext() 发出时，需要触发 onCompleted() 方法作为标志。

onError(): 事件队列异常。在事件处理过程中出异常时，onError() 会被触发，同时队列自动终止，不允许再有事件发出。

在一个正确运行的事件序列中, onCompleted() 和 onError() 有且只有一个，并且是事件序列中的最后一个。需要注意的是，onCompleted() 和 onError() 二者也是互斥的，即在队列中调用了其中一个，就不应该再调用另一个。


* subscrible

Observable 和 Observer 通过 subscribe() 方法实现订阅关系，从而 Observable 可以在需要的时候发出事件来通知 Observer。


* event

#### 1-3 RxJava2关于背压的调整

背压是指在异步场景中，被观察者发送事件速度远快于观察者的处理速度的情况下，一种告诉上游的被观察者降低发送速度的策略。

不支持背压的rxjava
Observable ( 被观察者 ) / Observer ( 观察者 )
支持背压的rxjava
Flowable （被观察者）/ Subscriber （观察者）

Single/SingleObserver
Completable/CompletableObserver
Maybe/MaybeObserver



* Observable 不支持背压的被观察者，发送0-n个数据-----和Observer配合使用
```java
  Observable mObservable=Observable.create(new ObservableOnSubscribe<Integer>() {
            @Override
            public void subscribe(ObservableEmitter<Integer> e) throws Exception {
                e.onNext(1);
                e.onNext(2);
                e.onComplete();
            }
        });
        
Observer mObserver=new Observer<Integer>() {
            //这是新加入的方法，在订阅后发送数据之前，
            //回首先调用这个方法，而Disposable可用于取消订阅
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onNext(Integer value) {

            }

            @Override
            public void onError(Throwable e) {

            }

            @Override
            public void onComplete() {

            }
        };
        
mObservable.subscribe(mObserver);
```

这种观察者模型是不支持背压的。

啥叫不支持背压呢？

当被观察者快速发送大量数据时，下游不会做其他处理，即使数据大量堆积，调用链也不会报MissingBackpressureException,消耗内存过大只会OOM

我在测试的时候，快速发送了100000个整形数据，下游延迟接收，结果被观察者的数据全部发送出去了，内存确实明显增加了，遗憾的是没有OOM。

所以，当我们使用Observable/Observer的时候，我们需要考虑的是，数据量是不是很大(官方给出以1000个事件为分界线，仅供各位参考)


* Flowable 支持背压的被观察者，发送0-n个数据--------和subscirber配合使用
```
        Flowable.range(0,10)
        .subscribe(new Subscriber<Integer>() {
            Subscription sub;
            //当订阅后，会首先调用这个方法，其实就相当于onStart()，
            //传入的Subscription s参数可以用于请求数据或者取消订阅
            @Override
            public void onSubscribe(Subscription s) {
                Log.w("TAG","onsubscribe start");
                sub=s;
                sub.request(1);
                Log.w("TAG","onsubscribe end");
            }

            @Override
            public void onNext(Integer o) {
                Log.w("TAG","onNext--->"+o);
                sub.request(1);
            }
            @Override
            public void onError(Throwable t) {
                t.printStackTrace();
            }
            @Override
            public void onComplete() {
                Log.w("TAG","onComplete");
            }
        });
```
Flowable是支持背压的，也就是说，一般而言，上游的被观察者会响应下游观察者的数据请求，下游调用request(n)来告诉上游发送多少个数据。这样避免了大量数据堆积在调用链上，使内存一直处于较低水平。
但是你必须指定背压的策略，以保证你创建的Flowable是支持背压的（这个在1.0的时候就很难保证，可以说RxJava2.0收紧了create()的权限）。

根据上面的代码的结果输出中可以看到，当我们调用subscription.request(n)方法的时候，不等onSubscribe()中后面的代码执行，就会立刻执行到onNext方法，因此，如果你在onNext方法中使用到需要初始化的类时，应当尽量在subscription.request(n)这个方法调用之前做好初始化的工作;

当然，这也不是绝对的，我在测试的时候发现，通过create（）自定义Flowable的时候，即使调用了subscription.request(n)方法，也会等onSubscribe（）方法中后面的代码都执行完之后，才开始调用onNext。

TIPS: 尽可能确保在request（）之前已经完成了所有的初始化工作，否则就有空指针的风险。



* Single  发送单一数据------和SingleObserver配合使用

* Completable 不发送任何数据，只处理onComplete和onError事件---和CompletableObserver配合使用

* Maybe  发射0-1个数据，要么成功，要么失败--------和MaybeObserver配合使用
//判断是否登陆
```java
Maybe.just(isLogin())
    //可能涉及到IO操作，放在子线程
    .subscribeOn(Schedulers.newThread())
    //取回结果传到主线程
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new MaybeObserver<Boolean>() {
            @Override
            public void onSubscribe(Disposable d) {

            }

            @Override
            public void onSuccess(Boolean value) {
                if(value){
                    ...
                }else{
                    ...
                }
            }

            @Override
            public void onError(Throwable e) {
                
            }

            @Override
            public void onComplete() {

            }
        });

```


## 二、RxJava2的各部分的定义和使用

### 1、观察者

### 1.1 有四种观察者
观察者有四个类：
* observer

```java
Observer<Apps> observer = new Observer<Apps>() {
        @Override
        public void onCompleted() {
            listView.onRefreshComplete();
        }

        @Override
        public void onError(Throwable e) {
            listView.onRefreshComplete();
        }

        @Override
        public void onNext(Apps appsList) {
            listView.onRefreshComplete();
            appLists.addAll(appsList.apps);
            adapter.notifyDataSetChanged();
        }
    };
```

* consumer

* action

* subscribler




### 2、被观察者

#### 2.1 有五种observable
在RxJava中，被观察者有五种形式
* Flowable 支持背压的被观察者，发送0-n个数据

* Observable 不支持背压的被观察者，发送0-n个数据

* Single  发送单一数据

* Completable 不发送任何数据，只处理onComplete和onError事件

* Maybe  发射0-1个数据，要么成功，要么失败



#### 2.2 创建observable

##### 2.2.1用create创建一个observer

```java
Observable observable = Observable.create(new ObservableOnSubscribe<String>() {
    @Override
    public void subscirbe(Subscriber<? super String> subscriber) {
        subscriber.onNext("Hello");
        subscriber.onNext("Hi");
        subscriber.onNext("Aloha");
        subscriber.onCompleted();
    }
});

```
分别看一下具体的定义：
```java
 @CheckReturnValue
    @SchedulerSupport(SchedulerSupport.NONE)
    public static <T> Observable<T> create(ObservableOnSubscribe<T> source) {
        ObjectHelper.requireNonNull(source, "source is null");
        return RxJavaPlugins.onAssembly(new ObservableCreate<T>(source));
    }
```
Observable类的静态方法create创建一个observable类的实例，该实例用一个ObservableOnSubscribe作为参数。再来看一下ObservableOnSubscribe的描述
```java
public interface ObservableOnSubscribe<T> {

    /**
     * Called for each Observer that subscribes.
     * @param e the safe emitter instance, never null
     * @throws Exception on error
     */
    void subscribe(ObservableEmitter<T> e) throws Exception;
}
```
ObservableOnSubscribe是一个接口，其中有一个方法subscribe需要实现。
再看看ObservableEmitter：

```java
public interface ObservableEmitter<T> extends Emitter<T> {

    /**
     * Sets a Disposable on this emitter; any previous Disposable
     * or Cancellation will be unsubscribed/cancelled.
     * @param d the disposable, null is allowed
     */
    void setDisposable(Disposable d);

    /**
     * Sets a Cancellable on this emitter; any previous Disposable
     * or Cancellation will be unsubscribed/cancelled.
     * @param c the cancellable resource, null is allowed
     */
    void setCancellable(Cancellable c);

    /**
     * Returns true if the downstream disposed the sequence.
     * @return true if the downstream disposed the sequence
     */
    boolean isDisposed();

    /**
     * Ensures that calls to onNext, onError and onComplete are properly serialized.
     * @return the serialized ObservableEmitter
     */
    ObservableEmitter<T> serialize();
}

```
Emitter是最后的接口：
```java
public interface Emitter<T> {

    /**
     * Signal a normal value.
     * @param value the value to signal, not null
     */
    void onNext(@NonNull T value);

    /**
     * Signal a Throwable exception.
     * @param error the Throwable to signal, not null
     */
    void onError(@NonNull Throwable error);

    /**
     * Signal a completion.
     */
    void onComplete();
}

```
通过单一接口函数SAM的lambda转换，可以变为：
```java
Observable.create { aSubscriber ->
        50.times { i ->
            if (!aSubscriber.unsubscribed) {
                aSubscriber.onNext("value_${i}")
            }
        }
        // after sending all values we complete the sequence
        if (!aSubscriber.unsubscribed) {
            aSubscriber.onCompleted()
        }
    }

```
##### 2.2.2用interval方法创建一个Observable
 //  从0开始 ，每隔1秒发送一个整数 (默认执行无数次 使用`take(int)`方法限制执行次数)
    val disposable = Observable.interval(0, 1, TimeUnit.SECONDS)
        .take(5)
        .subscribe { print("$it  ") }
    if (!disposable.isDisposed) disposable.dispose()
##### 2.2.3 用defer创建一个Observable
```java
/**
 * 使用`defer()`方法创建事件序列
 *
 * `defer`直到有观察者订阅时才创建Observable，并且为每个观察者创建一个新的Observable
 * `defer`操作符会一直等待直到有观察者订阅它，然后它使用Observable工厂方法生成一个Observable
 */
fun defer() {
    print("[defer]: ")

    val observable =
        Observable.defer { Observable.just(System.currentTimeMillis()) }
    observable.subscribe { print("$it ") }   // 454  订阅时才产生了Observable
    print("   ")
    observable.subscribe { print("$it ") }   // 459  订阅时才产生了Observable

    println()
}

```
##### 2.2.4 使用`empty()` `never()` `error()`方法创建事件序列
```java
/**
 * 使用`empty()` `never()` `error()`方法创建事件序列
 *
 * public static <T> Observable<T> `empty()`：创建一个不发射任何数据但是正常终止的Observable
 * public static <T> Observable<T> `never()`：创建一个不发射数据也不终止的Observable
 * public static <T> Observable<T> `error(Throwable exception)`：创建一个不发射数据以一个错误终止的Observable
 */
fun emptyNeverError() {
    print("[empty]: ")

    Observable.empty<String>().subscribeBy(
        onNext = { print(" next ") },
        onComplete = { print(" complete ") },
        onError = { print(" error ") }
    )
    // `empty()`只会调用onComplete方法

    println()

    print("[never]: ")

    Observable.never<String>().subscribeBy(
        onNext = { print(" next ") },
        onComplete = { print(" complete ") },
        onError = { print(" error ") }
    )
    // 什么也不会做

    println()

    print("[error]: ")

    Observable.error<Exception>(Exception()).subscribeBy(
        onNext = { print(" next ") },
        onComplete = { print(" complete ") },
        onError = { print(" error ") }
    )
    // `error()`只会调用onError方法

    println()
}
```
##### 2.2.5用repeat方法创建事件序列
```java
/**
 * 使用`repeat()`方法创建事件序列
 *
 * 表示指定的序列要发射多少次
 */
fun repeat() {
    // 重载方法
    // public final Observable<T> repeat(long times)
    // public final Observable<T> repeatUntil(BooleanSupplier stop)
    // public final Observable<T> repeatWhen(Function<? super Observable<Object>, ? extends ObservableSource<?>> handler)

    print("[repeat]: ")

    // 不指定次数即无限次发送(内部调用有次数的重载方法并传入Long.MAX_VALUE)  别执行啊 ~ 卡的不行 ~
    // Observable.range(5, 10).repeat().subscribe { print("$it  ") }

    Observable.range(5, 10).repeat(1).subscribe { print("$it  ") }

    println()

    print("[repeatUntil]: ")

    // repeatUntil在满足指定要求的时候停止重复发送，否则会一直发送
    // 这里当随机产生的数字`<10`时停止发送 否则继续  (这里始终为true(即停止重复) 省的疯了似的执行)
    val numbers = arrayOf(0, 1, 2, 3, 4)
    numbers.toObservable().repeatUntil {
        Random(10).nextInt() < 10
    }.subscribe { print("$it  ") }

    println()
}

```
##### 2.2.6用timer方法创建事件序列
```java
/**
 * 使用`timer()`方法创建事件序列
 *
 * 执行延时任务
 *
 * 创建一个在给定的时间段之后返回一个特殊值的Observable，它在延迟一段给定的时间后发射一个简单的数字0
 */
fun timer() {
    print("[timer]: ")

    // 在500毫秒之后输出一个数字0
    val disposable = Observable.timer(500, TimeUnit.MILLISECONDS).subscribe { print("$it  ") }
    if (!disposable.isDisposed) disposable.dispose()

    println()
}
```
##### 2.2.7用from方法创建事件序列
```java
/**
 * 使用`from()`方法快捷创建事件队列
 *
 * `fromArray`
 * `fromCallable`
 * `fromIterable`  (和上面的fromArray类似 只不过这里是集合罢了)
 *
 * 将传入的数组依次发送出来
 */
fun from() {
    print("[fromArray]: ")

    val names = arrayOf("ha", "hello", "yummy", "kt", "world", "green", "delicious")
    // 注意：使用`*`展开数组
    val disposable = Observable.fromArray(*names).subscribe { print("$it  ") }
    if (!disposable.isDisposed) disposable.dispose()

    println()

    print("[fromCallable]: ")

    // 可以在Callable内执行一段代码 并返回一个值给观察者
    Observable.fromCallable { 1 }.subscribe { print("$it  ") }

    println()
}
```
##### 2.2.8用just方法创建事件序列
```java
/**
 * 使用`just()`方法快捷创建事件队列
 *
 * 将传入的参数依次发送出来(最少1个 最多10个)
 */
fun just() {
    print("[just]: ")

    val disposable = Observable.just("Just1", "Just2", "Just3")
        // 将会依次调用：
        // onNext("Just1");
        // onNext("Just2");
        // onNext("Just3");
        // onCompleted();
        .subscribe { print("$it  ") }
    if (!disposable.isDisposed) disposable.dispose()

    println()
}

```java

##### 2.2.8用just方法创建事件序列
```java
/**
 * 使用`range()`方法快捷创建事件队列
 *
 * 创建一个序列
 */
fun range() {
    print("[range]: ")

    // 用Observable.range()方法产生一个序列
    // 用map方法将该整数序列映射成一个字符序列
    // 最后将得到的序列输出来 forEach内部也是调用的 subscribe(Consumer<? super T> onNext)
    val disposable = Observable.range(0, 10)
        .map { item -> "range$item" }
//        .forEach { print("$it  ") }
        .subscribeBy(
            onNext = { print("$it  ") },
            onComplete = { print("range complete !!! ") }
        )
    if (!disposable.isDisposed) disposable.dispose()

    println()
}

```
#### 2.3 转换observable

##### 2.3.1 用map和cast做转换操作
```java
/**
 * 使用`map()` `cast()` 做变换操作
 *
 * `map`操作符对原始Observable发射的每一项数据应用一个你选择的函数，然后返回一个发射这些结果的Observable。默认不在任何特定的调度器上执行
 *
 * `cast`操作符将原始Observable发射的每一项数据都强制转换为一个指定的类型`（多态）`，然后再发射数据，它是map的一个特殊版本
 */
fun mapCast() {
    print("[map]: ")

    Observable.range(1, 5).map { item -> "to String $item" }.subscribe { print("$it  ") }

    println()

    print("[cast]: ")

    // 将`Date`转换为`Any` (如果前面的Class无法转换成第二个Class就会出现ClassCastException)
    Observable.just(Date()).cast(Any::class.java).subscribe { print("$it  ") }

    println()
}

/*
    `map`与`flatMap`的区别(出自朱凯):
    `map`是在一个 item 被发射之后，到达 map 处经过转换变成另一个 item ，然后继续往下走；
    `flapMap`是 item 被发射之后，到达 flatMap 处经过转换变成一个 Observable
    而这个 Observable 并不会直接被发射出去，而是会立即被激活，然后把它发射出的每个 item 都传入流中，再继续走下去。
 */
```
##### 2.3.2 用flatmap做转换

```java
/**
 * 使用`flatMap()` `contactMap()` 做变换操作
 *
 * `flatMap`将一个发送事件的上游Observable变换为多个发送事件的Observables，然后将它们发射的事件合并后放进一个单独的Observable里
 * `flatMap`不保证顺序  `contactMap()`保证顺序
 */
fun flatMap2contactMap() {
    print("[flatMap]: ")

    /*
        `flatMap()` 的原理是这样的：
        1. 使用传入的事件对象创建一个 Observable 对象；
        2. 并不发送这个 Observable, 而是将它激活，于是它开始发送事件；
        3. 每一个创建出来的 Observable 发送的事件，都被汇入同一个 Observable ，
        而这个 Observable 负责将这些事件统一交给 Subscriber 的回调方法。
        这三个步骤，把事件拆成了两级，通过一组新创建的 Observable 将初始的对象『铺平』之后通过统一路径分发了下去。
        而这个『铺平』就是 flatMap() 所谓的 flat。
     */

    Observable.range(1, 5).flatMap {
        Observable.just("$it to flat")
    }.subscribe { print("$it  ") }

    println()

    print("[contactMap]: ")

    Observable.range(1, 5).concatMap {
        Observable.just("$it to concat")
    }.subscribe { print("$it  ") }

    println()

}

/**
 * `flatMap`挺重要的，再举一个例子
 *
 * 依次打印Person集合中每个元素中Plan的action
 */
fun flatMapExample() {
    print("[flatMapExample]: ")

    arrayListOf(
        Person("KYLE", arrayListOf(Plan(arrayListOf("Study RxJava", "May 1 to go home")))),
        Person("WEN QI", arrayListOf(Plan(arrayListOf("Study Java", "See a Movie")))),
        Person("LU", arrayListOf(Plan(arrayListOf("Study Kotlin", "Play Game")))),
        Person("SUNNY", arrayListOf(Plan(arrayListOf("Study PHP", "Listen to music"))))
    ).toObservable().flatMap {
        Observable.fromIterable(it.planList)
    }.flatMap {
        Observable.fromIterable(it.actionList)
    }.subscribeBy(
        onNext = { print("$it  ") }
    )

    println()
}

/**
 * 使用`flatMapIterable()`做变换操作
 *
 * 将上流的任意一个元素转换成一个Iterable对象
 */
fun flatMapIterable() {
    print("[flatMapIterable]: ")

    Observable.range(1, 5)
        .flatMapIterable { integer ->
            Collections.singletonList("$integer")
        }
        .subscribe { print("$it  ") }

    // [flatMapIterable]: 1  2  3  4  5

    println()
}
```
##### 2.3.3用buffer做变换，用来分组
```java
/**
 * 使用`buffer()`做变换操作
 *
 * 用于将整个流进行分组
 */
fun buffer() {
    print("[buffer]: ")

    // 生成一个7个整数构成的流，然后使用`buffer`之后，这些整数会被3个作为一组进行输出

    // count 是一个buffer的最大值
    // skip 是指针后移的距离(不定义时就为count)
    // 例如 1 2 3 4 5 buffer(3) 的结果为：[1,2,3] [4,5]      (buffer(3)也就是buffer(3,3))
    // 例如 1 2 3 4 5 buffer(3,2) 的结果为：[1,2,3] [3,4,5] [5]
    Observable.range(1, 5).buffer(3)
        .subscribe {
            print("${Arrays.toString(it.toIntArray())}  ")
        }

    println()
}


```

##### 2.3.4用groupby做变换
```java
/**
 * 使用`groupBy()`做变换操作
 *
 * 用于分组元素(根据groupBy()方法返回的值进行分组)
 * 将发送的数据进行分组，每个分组都会返回一个被观察者。
 */
fun groupBy() {
    print("[groupBy]: ")

    // 使用concat方法先将两个Observable拼接成一个Observable，然后对其元素进行分组。
    // 这里我们的分组依据是整数的值，这样我们将得到一个Observable<GroupedObservable<Integer, Integer>>类型的Observable。
    // 然后，我们再将得到的序列拼接成一个，并进行订阅输出

    Observable.concat(Observable.concat(
        Observable.range(1, 3), Observable.range(1, 4)
    ).groupBy { it }
    ).subscribe { print("groupBy: $it  ") }

    // [groupBy]: groupBy: 1  groupBy: 1  groupBy: 2  groupBy: 2  groupBy: 3  groupBy: 3  groupBy: 4

    println()
}
```

##### 2.3.5用scan做变换
```java
/**
 * 使用`scan()`做变换操作
 *
 * 将数据以一定的逻辑聚合起来
 *
 * scan操作符对原始Observable发射的第一项数据应用一个函数，然后将那个函数的结果作为自己的第一项数据发射。
 * 它将函数的结果同第二项数据一起填充给这个函数来产生它自己的第二项数据。
 * 它持续进行这个过程来产生剩余的数据序列。这个操作符在某些情况下被叫做`accumulator`
 */
fun scan() {
    print("[scan]: ")

    val disposable = Observable.just(1, 2, 3, 4, 5)
        .scan { t1: Int, t2: Int -> t1 + t2 }
        .subscribe { print("$it  ") }
    if (!disposable.isDisposed) disposable.dispose()

    // [scan]: 1  3  6  10  15

    println()
}
```

##### 2.3.6用window做变换操作
```java
/**
 * 使用`window()`做变换操作
 *
 * 将事件分组 参数`count`就是分的组数
 *
 * `window`和`buffer`类似，但不是发射来自原始Observable的数据包，它发射的是Observable，
 * 这些Observables中的每一个都发射原始Observable数据的一个子集，最后发射一个onCompleted通知。
 */
fun window() {
    print("[window]: ")

    Observable.range(1, 10).window(3)
        .subscribeBy(
            onNext = { it.subscribe { int -> print("{${it.hashCode()} : $int} ") } },
            onComplete = { print("onComplete ") }
        )

    println()
}
```

#### 2.4 过滤操作符

    // ---------------- `Observable` 过滤操作/条件操作符 ----------------
    println("---------------- `Observable` 过滤操作/条件操作符 ----------------")
    filter()
    element()
    distinct()
    skip()
    take()
    ignoreElements()
    debounce()
    ofType()
    all()
    contains()
    isEmpty()
    defaultIfEmpty()
    amb()
    println()

#### 2.5 组合操作符
    // ---------------- `Observable` 组合操作 ----------------
    println("---------------- `Observable` 组合操作 ----------------")
    concat()
    merge()
    startWith()
    zip()
    combineLast()
    reduce()
    collect()
    count()
    println()

#### 2.6 辅助操作符
    // ------------------- 功能操作符/辅助操作 -------------------
    println("------------------- 功能操作符/辅助操作 -------------------")
    delay()
    doSeries()
    retry()
    subscribeOn()
    observeOn()
    println()

#### 2.7 扩展操作符
    // ---------------- RxKotlin扩展库 ----------------
    println("---------------- RxKotlin扩展库 ----------------")
    rkExExample()
    println()

#### 2.8 其他操作符
##### 2.8.1 compose 操作符
```java
  /**
 * `compose`
 * 与`Transformer`连用
 *
 * `compose`操作符和Transformer结合使用，一方面让代码看起来更加简洁化，另一方面能够提高代码的复用性。
 * RxJava提倡链式调用，`compose`能够防止链式被打破。
 *
 * compose操作于整个数据流中，能够从数据流中得到原始的Observable<T>/Flowable<T>...
 * 当创建Observable/Flowable...时，compose操作符会立即执行，而不像其他的操作符需要在onNext()调用后才执行
 */
fun compose() {
    println("[compose]: ")

    Observable.just(1, 2)
        .compose(transformerInt2String())
        .compose(applySchedulers())
        .subscribe {
            print("$it  ")
            if (it == "1") print(" ${Thread.currentThread().name} ")
        }

    println()
}
```

#### 2.9 和回调相关的操作符
```java

3、doOnEach
数据源（Observable）每发送一次数据，就调用一次。
4、doOnNext
数据源每次调用onNext() 之前都会先回调该方法。
5、doOnError
数据源每次调用onError() 之前会回调该方法。
6、doOnComplete
数据源每次调用onComplete() 之前会回调该方法
7、doOnSubscribe
数据源每次调用onSubscribe() 之后会回调该方法
8、doOnDispose
数据源每次调用dispose() 之后会回调该方法
```
#### 2.10 错误处理操作符
```java
错误处理操作符
1、onErrorReturn
作用于Flowable、Observable、Maybe、Single。但调用数据源的onError函数后会回到该函数，可对错误进行处理，然后返回值，会调用观察者onNext()继续执行，执行完调用onComplete()函数结束所有事件的发射。
Single.just("2A")
    .map(v -> Integer.parseInt(v, 10))
    .onErrorReturn(error -> {
        if (error instanceof NumberFormatException) return 0;
        else throw new IllegalArgumentException();
    })
    .subscribe(
        System.out::println,
        error -> System.err.println("onError should not be printed!"));

// prints 0
复制代码2、onErrorReturnItem
与onErrorReturn类似，onErrorReturnItem不对错误进行处理，直接返回一个值。
Single.just("2A")
    .map(v -> Integer.parseInt(v, 10))
    .onErrorReturnItem(0)
    .subscribe(
        System.out::println,
        error -> System.err.println("onError should not be printed!"));

// prints 0
复制代码3、onExceptionResumeNext
可作用于Flowable、Observable、Maybe。onErrorReturn发生异常时，回调onComplete()函数后不再往下执行，而onExceptionResumeNext则是要在处理异常的时候返回一个数据源，然后继续执行，如果返回null，则调用观察者的onError()函数。
Observable.create((ObservableOnSubscribe<Integer>) e -> {
            e.onNext(1);
            e.onNext(2);
            e.onNext(3);
            e.onError(new NullPointerException());
            e.onNext(4);
        })
                .onErrorResumeNext(throwable -> {
                    Log.d(TAG, "onErrorResumeNext ");
                    return Observable.just(4);
                })
                .subscribe(new Observer<Integer>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "onSubscribe ");
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "onNext " + integer);
                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "onError ");
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "onComplete ");
                    }
                });
复制代码结果：

onExceptionResumeNext操作符也是类似的，只是捕获Exception。
4、retry
可作用于所有的数据源，当发生错误时，数据源重复发射item，直到没有异常或者达到所指定的次数。
boolean first=true;

Observable.create((ObservableOnSubscribe<Integer>) e -> {
            e.onNext(1);
            e.onNext(2);

            if (first){
                first=false;
                e.onError(new NullPointerException());

            }
            
        })
                .retry(9)
                .subscribe(new Observer<Integer>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "onSubscribe ");
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "onNext " + integer);

                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "onError ");
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "onComplete ");
                    }
                });
复制代码结果：

5、retryUntil
作用于Flowable、Observable、Maybe。与retry类似，但发生异常时，返回值是false表示继续执行(重复发射数据)，true不再执行,但会调用onError方法。
 Observable.create((ObservableOnSubscribe<Integer>) e -> {
            e.onNext(1);
            e.onNext(2);
            e.onError(new NullPointerException());
            e.onNext(3);
            e.onComplete();
        })
                .retryUntil(() -> true)
                .subscribe(new Observer<Integer>() {
                    @Override
                    public void onSubscribe(Disposable d) {
                        Log.d(TAG, "onSubscribe ");
                    }

                    @Override
                    public void onNext(Integer integer) {
                        Log.d(TAG, "onNext " + integer);

                    }

                    @Override
                    public void onError(Throwable e) {
                        Log.d(TAG, "onError ");
                    }

                    @Override
                    public void onComplete() {
                        Log.d(TAG, "onComplete ");
                    }
                });
复制代码结果：

retryWhen与此类似，但其判断标准不是BooleanSupplier对象的getAsBoolean()函数的返回值。而是返回的 Observable或Flowable是否会发射异常事件。

```
### 3、线程调度

在不指定线程的情况下， RxJava 遵循的是线程不变的原则，即：在哪个线程调用 subscribe()，就在哪个线程生产事件；在哪个线程生产事件，就在哪个线程消费事件。如果需要切换线程，就需要用到 Scheduler （调度器）

在RxJava 中，Scheduler ——调度器，相当于线程控制器，RxJava 通过它来指定每一段代码应该运行在什么样的线程。RxJava 已经内置了几个 Scheduler ，它们已经适合大多数的使用场景：

* Schedulers.immediate(): 

直接在当前线程运行，相当于不指定线程。这是默认的 Scheduler。

* Schedulers.newThread(): 

总是启用新线程，并在新线程执行操作。

* Schedulers.io(): I/O 

操作（读写文件、读写数据库、网络信息交互等）所使用的 Scheduler。行为模式和 newThread() 差不多，区别在于 io() 的内部实现是是用一个无数量上限的线程池，可以重用空闲的线程，因此多数情况下 io() 比 newThread() 更有效率。不要把计算工作放在 io() 中，可以避免创建不必要的线程。

* Schedulers.computation(): 

计算所使用的 Scheduler。这个计算指的是 CPU 密集型计算，即不会被 I/O 等操作限制性能的操作，例如图形的计算。这个 Scheduler 使用的固定的线程池，大小为 CPU 核数。不要把 I/O 操作放在 computation() 中，否则 I/O 操作的等待时间会浪费 CPU。

* Android 还有一个专用的 AndroidSchedulers.mainThread()，
它指定的操作将在 Android 主线程运行。


有了这几个 Scheduler ，就可以使用 subscribeOn() 和 observeOn() 两个方法来对线程进行控制了。 

``subscribeOn()`` 指定 subscribe() 所发生的线程，即 Observable.OnSubscribe 被激活时所处的线程。或者叫做事件产生的线程。 

``observeOn()`` 指定 Subscriber 所运行在的线程。或者叫做事件消费的线程。
```java
Observable.just(1, 2, 3, 4)
    .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
    .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
    .subscribe(new Action1<Integer>() {
        @Override
        public void call(Integer number) {
            Log.d(tag, "number:" + number);
        }
    });

```
上面这段代码中，由于 subscribeOn(Schedulers.io()) 的指定，被创建的事件的内容 1、2、3、4 将会在 IO 线程发出；

而由于 observeOn(AndroidScheculers.mainThread()) 的指定，因此 subscriber 数字的打印将发生在主线程 。

事实上，这种在 subscribe() 之前写上两句 subscribeOn(Scheduler.io())和 observeOn(AndroidSchedulers.mainThread()) 的使用方式非常常见，它适用于多数的 『后台线程取数据，主线程显示』的程序策略。


而前面提到的由图片 id 取得图片并显示的例子，如果也加上这两句：
```java
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
    @Override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDrawable(drawableRes));
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
})
.subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
.observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
.subscribe(new Observer<Drawable>() {
    @Override
    public void onNext(Drawable drawable) {
        imageView.setImageDrawable(drawable);
    }

    @Override
    public void onCompleted() {
    }
 
    @Override
    public void onError(Throwable e) {
        Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
    }
});
```
那么，加载图片将会发生在 IO 线程，而设置图片则被设定在了主线程。这就意味着，即使加载图片耗费了几十甚至几百毫秒的时间，也不会造成丝毫界面的卡顿。


























































































