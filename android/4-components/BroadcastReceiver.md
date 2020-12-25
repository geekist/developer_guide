

# BroadcastReceiver

需要掌握：

**1、Broadcast概念及工作原理**

**2、Broadcast分类、定义和发送**

**3、BroadcastReceiver的定义、注册和反注册**

## 广播机制及原理

Android中的广播使用了观察者模式，基于消息的发布/订阅事件模型。将广播的发送者和接受者极大程度上解耦，使得系统能够方便集成，更易扩展。

实现流程：

1.广播接收者BroadcastReceiver通过Binder机制向AMS(Activity Manager Service)进行注册；

2.广播发送者通过binder机制向AMS发送广播；

3.AMS查找符合相应条件（IntentFilter/Permission等）的BroadcastReceiver，将广播发送到BroadcastReceiver（一般情况下是Activity）相应的消息循环队列中；

4.消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive()方法。

## Broadcast分类、定义和发送

### 按照broadcast的类型来分，可以分为标准广播和有序广播（sticky广播被废弃）
* staandard broadcast  
标准广播是一种完全异步执行的广播，发出后所有注册了该广播的接受者几乎会在同一时间内接收到该广播消息，这个消息没有先后顺序，不可以被阻截。  

* ordered broadcast   
有序广播是一种同步执行的广播，广播发出后，同一时刻只有一个接受者能够收到这条广播消息。接受者根据优先级来判定。当该广播消息被处理完成后广播才会继续向下传递，而且接受者可以截断广播，这样后面的接受者就无法接受广播消息了。

### 定义和发送一个Broadcast ---- 通过Intent
```kotlin
val intent = Intent("com.example.broadcastTest.MY_BROADCAST") //消息名
intent.setPackage(packageName)  //显示指定消息的接受对象
sendBroadcast(intent)
sendOrderedBroadcast(intent,null)
```

## BroadcastReceiver的定义、注册和反注册

### 创建一个BroadcastReceiver

首先创建一个BroadcastReceiver的子类，实现BroadcastReceiver的onReceive功能

```kotlin
public class MyBroadcastReceiver extends BroadcastReceiver {
    public static final String TAG = "MyBroadcastReceiver";
    public static int m = 1;

    @Override
    public void onReceive(Context context, Intent intent) {
        Log.w(TAG, "intent:" + intent);
        String name = intent.getStringExtra("name");
        Log.w(TAG, "name:" + name + " m=" + m);
        m++;

        Bundle bundle = intent.getExtras();

    }
}
```

### 注册BroadcastReceiver的两种方法：

**静态注册**

在Android.manifest中注册receiver，指明接受广播的类型
```kotlin
        <receiver android:name=".BootCompleteReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
               ` <action android:name="android.intent.action.BOOT_COMPLETED">`
                </action>
            </intent-filter>
```
用静态方式注册的receiver，只能接受一部分的系统消息，且需要在manifest中申请权限
```kotlin
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED">
```
注：上述的例子在小米手机没有测试成功

**动态注册--context的register和unregister方法** 

用代码创建一个Intent来注册receiver 
```kotlin

class MainActivity : AppCompatActivity() {

    lateinit var  timeTickReceiver: TimeTickReceiver

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        Log.d("MainActivity","thread is :" + Thread.currentThread().name)

        val intentFilter = IntentFilter()
        `intentFilter.addAction("android.intent.action.TIME_TICK")`
        timeTickReceiver = TimeTickReceiver()
        registerReceiver(timeTickReceiver,intentFilter)
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(timeTickReceiver)
    }

    inner class TimeTickReceiver: BroadcastReceiver() {
        override fun onReceive(context: Context?, intent: Intent?) {
            Log.d("MainActivity_receiver","thread is :" + Thread.currentThread().name)
        }
    }
}

```

### 注册之后要反注册
```java
public class MainActivity extends Activity {
    public static final String BROADCAST_ACTION = "com.example.corn";
    private BroadcastReceiver mBroadcastReceiver;

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unregisterReceiver(mBroadcastReceiver);
    }

}

```
