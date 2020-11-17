# EventBus

## github地址：https://github.com/greenrobot/EventBus

## 1、Kotlin依赖项添加：
```gradle
dependencies {
    implementation 'org.greenrobot:eventbus:3.2.0'
}
```

## 2、ProGuard
```java
 -keepattributes *Annotation*
-keepclassmembers class * {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }
 
#And if you use AsyncExecutor:
-keepclassmembers class * extends org.greenrobot.eventbus.util.ThrowableFailureEvent {
    <init>(java.lang.Throwable);
}
```

## 3、如何使用EventBus
* 1、定义事件
```java
public class MessageEvent {
    public final String message;
    public MessageEvent(String message) {
        this.message = message;
    }
}
```
* 2、注册和反注册订阅事件

```java
@Override
public void onStart() {
    super.onStart();
    EventBus.getDefault().register(this);
}

@Override
public void onStop() {
    EventBus.getDefault().unregister(this);
    super.onStop();
}

```
* 3、订阅事件

```java
// This method will be called when a MessageEvent is posted (in the UI thread for Toast)
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessageEvent(MessageEvent event) {
    Toast.makeText(getActivity(), event.message, Toast.LENGTH_SHORT).show();
}

// This method will be called when a SomeOtherEvent is posted
@Subscribe
public void handleSomethingElse(SomeOtherEvent event) {
    doSomethingWith(event);
}
```

* 4、发送事件消息

```java
EventBus.getDefault().post(new MessageEvent("Hello everyone!"));
```

## 4、五种线程模式
* ThreadMode:POSTING

表示事件在哪个线程中发布出来的，事件订阅函数onEvent就会在这个线程中运行，也就是说发布事件和接收事件在同一个线程。就是子线程发布->子线程接收，主线程发布->主线程接收。

* ThreadMode：MAIN 

表示无论事件是在UI线程（主线程）或者子线程发布出来的，该事件订阅方法onEvent都会在UI线程中执行。就是子线程发布->主线程接收，主线程发布->主线程接收。

* ThreadMode：MAIN_ORDER
* ThreadMode：BACKGROUND

表示如果事件在UI线程中发布出来的，那么订阅函数onEvent就会在子线程中运行，如果事件本来就是在子线程中发布出来的，那么订阅函数直接在该子线程中执行。也就是说主线程发布->子线程接收、子线程发布->子线程接收

* ThreadMode：ASYNC

使用这个模式的订阅函数，那么无论事件在哪个线程发布，都会创建新的子线程来执行订阅函数。也就是说主线程发布->创建新的子线程接收、子线程发布->创建新的子线程接收（两个子线程不同）。

## 5、粘连事件
* 普通事件：通过post()方法发出的普通事件，会被已经注册的订阅者接收到，若订阅者是在消息发送之后才注册，那么是不会接收到该事件的

* 粘性事件：而粘性事件是可以被事件发出之后才注册的订阅者接收到，也可以在事件发出之后通过主动查询获取事件内容。粘性事件实现原理其实是把最近的事件缓存到内存中，之后注册的订阅者还可以查询出来

比如在AActivity中发送一个粘性事件Event，然后打开BActivity，在BActivity中注册Event的粘性事件订阅者，在注册后马上可以接收到该事件。而如果是普通事件的话是接收不到的，因为订阅者是在消息发送之后才注册的。这就是粘性事件的方便之处。

* 主动获取及移除粘性事件
```java
MessageEvent stickyEvent = EventBus.getDefault().getStickyEvent(MessageEvent.class);

```

```java
if(stickyEvent != null) {
    // "Consume" the sticky event
    EventBus.getDefault().removeStickyEvent(stickyEvent);
    // Now do something with it
}

```
## 6、优先级和取消事件
priority表示优先级，数值越大，优先级越高。在同一传递线程（ThreadMode）中，较高优先级的订户将在优先级较低的其他订户之前接收事件。默认优先级为0。

```java
@Subscribe(priority = 1);
public void onEvent(MessageEvent event) {
   //do something
}
```


事件的优先级越高接收的数据最快，所以当优先级不想分发事件给低级别的事件时，可以使用cancelEventDelivery （Object event）这里的参数是订阅的实体参数。如下代码。

注：当优先级更高的想取消事件传递时，只有当threadMode = ThreadMode.POSTING处于此状态才能取消事件传递有效。其他不行

```java
@Subscribe(priority = 1000,threadMode = ThreadMode.POSTING)
public void onEvent(MessageEvent event){
     textView.setText(event.message);//设置接收的数据
    //取消事件传递，则低级别的事件无法接收到信息，只有在threadMode = ThreadMode.POSTING情况下
    EventBus.getDefault().cancelEventDelivery(event) ;
}

```

## 7、事件索引
订户索引是EventBus 3的新功能。它是一种可选的优化，可加快初始订户注册。可以使用EventBus批注处理器在构建期间创建订户索引。虽然不需要使用索引，但建议在Android上获得最佳性能。
当EventBus无法使用索引时，它将在运行时自动回退到反射。因此它仍然可以工作，只是有点慢。所以添加索引可以让EventBus的使用效率更高。

## 8、用EventBusBuilder来配置EventBus
EventBusBuilder类配置EventBus的各个方面事项。例如：以下是如何构建一个在发布的事件没有订阅者的情况下保持静态的EventBus：
```java
EventBus eventBus = EventBus.builder()
    .logNoSubscriberMessages(false)
    .sendNoSubscriberEvent(false)
    .build();
```






