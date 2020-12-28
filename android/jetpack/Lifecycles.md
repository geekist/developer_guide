

# Lifecycles
一个观察者模式的应用场景。

Activiry或者Fragment作为一个obserable，以lifecycle存在于其自身。
创建一个observer，用来订阅observerable的变化
lifecycle.addObserver(observer)将二者结合起来，这样，当前者变化时，后者就能够受到消息


## 1、添加依赖项
```java

    implementation 'androidx.lifecycle:lifecycle-extensions:2.2.0'

```
## 2、创建一个Observer，实现LifecycleObserver接口
```java
class MyObserver(val lifecycle: Lifecycle) : LifecycleObserver {

    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun activityStart() {
        Log.d("MyObserver","activity Started")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun activityStop() {
        Log.d("MyObserver","activity Stopped")
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_CREATE)
    fun activityCreated() {
        Log.d("MyObserver","activity Created")
    }
    @OnLifecycleEvent(Lifecycle.Event.ON_PAUSE)
    fun activityPause() {
        Log.d("MyObserver","activity Paused")
    }
}
```	
## 3、在Activity或fragment中实现对生命周期的观察
因为activity和fragment
```java
  override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        lifecycle.addObserver(MyObserver(lifecycle))
}


```		
