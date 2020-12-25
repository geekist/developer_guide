
# Service

需要掌握：

**1、service概念**

**2、如何定义和使用一个service**

**3、Service和Activity的通信**

**4、前台service**

**5、IntentService**



## Service概述
Service是android中实现程序后台运行的解决方案，适用于不需要和用户交互但是要求长期运行的任务。

service的运行不依赖用户界面，当程序切换到后台时，service仍然可以运行，直到当应用被杀掉时，所有依赖于该应用的service会停止。

sevice是运行于ui线程的，除非在servcie中手动添加线程使某些代码在后台运行。

## Service的使用方法

### 1、定义一个service类
定义一个service类，必须实现onBind抽象方法。其他的onCreate，onStart，onDestroy
可以重载。

```kotlin

class MyService : Service() {
    private val TAG = MyService::class.java.simpleName

    private val mBinder = MyBinder()

    class MyBinder : Binder() {
        fun download() {
            Log.d("MyService.TAG", " download")
        }

        fun progress() {
            Log.d("MyService.TAG", "start progress")
        }
    }

	//必须实现的抽象方法
    override fun onBind(intent: Intent): IBinder {
        return mBinder
        // Log.d(TAG,"on Bind")
        // return null
    }

	可以重载的方法
    override fun onCreate() {
        super.onCreate()
    }

	可以重载的方法
    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Log.d(TAG, "on start")
        return super.onStartCommand(intent, flags, startId)
    }

	可以重载的方法
    override fun onDestroy() {
        super.onDestroy()
        Log.d(TAG, "on destroy")

    }
}

```
### 2、在mainifest文件中注册service

同时，manifest文件中要注册这个service
```xml
  <service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>
```

### 3、启动一个service

在activity或fragment界面中调用startService

```kotlin
 val intent = Intent(this,MyService::class.java)
 startService(intent)
```
### 4、停止一个service

```kotlin
 val intent = Intent(this,MyService::class.java)
 stopService(intent)
```

## Activity和Service通信

通过在service端定义binder类，在activity端通过connection来得到这个binder实现。

### 1、定义一个service，其中包含一个Binder类,binder类中可以执行一些操作。
```java

class MyService : Service() {
    private val TAG = MyService::class.java.simpleName

    private val mBinder = MyBinder()

    class MyBinder : Binder() {
        fun download() {
            Log.d("MyService.TAG", " download")
        }

        fun progress() {
            Log.d("MyService.TAG", "start progress")
        }
    }

    override fun onBind(intent: Intent): IBinder {
        return mBinder
    }

    override fun onCreate() {
        super.onCreate()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        return super.onStartCommand(intent, flags, startId)
    }

    override fun onDestroy() {
        super.onDestroy()
    }
}

```
### 2、在activity中定义一个ServiceConnection类

ServiceConnection类实现一个包含Binder参数的抽象方法，可以在connection类中调用。
```java

    lateinit var downloadBinder: MyService.MyBinder
    private val connection = object: ServiceConnection {
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            downloadBinder = service as MyService.MyBinder
            downloadBinder.download()
            downloadBinder.progress()
        }

        override fun onServiceDisconnected(name: ComponentName?) {
            TODO("Not yet implemented")
        }
    }

```

### 3、通过bindService将activity和service联系起来
在activity和fragment中调用bindService和unbindService
```kotlin
  buttonBindService.setOnClickListener{
            val intent = Intent(this,MyService::class.java)
            bindService(intent,connection, Context.BIND_AUTO_CREATE)
        }
```
### 4、取消activity和service的绑定

```java
        buttonUnBindService.setOnClickListener{
            unbindService(connection)
        }

```


## Service的生命周期

## 前台service
从android 8 开始，系统对于后台service有了严格的限制，进入后台的service，随时可能被系统回收。

如果要保持service的运行，需要使用前台service，即以通知的方式将service显示在系统状态栏中，


[notification复习一下先](https://github.com/geekist/developer_guide/blob/main/android/notification/Notification.md)

### 前台service的创建

在service的onCreate函数中，添加一个通知，然后startForeground

```kotlin
class MyService: service() {
override fun onCreate() {
        super.onCreate()
        Log.d(TAG, "on create")

//step1: 创建一个消息通道（8以上）

        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                "my_service",
                "qiantai service",
                NotificationManager.IMPORTANCE_DEFAULT
            )
            manager.createNotificationChannel(channel)
        }

//step2： 创建一个pendingIntent，即只有点击了消息才能启动的intent

        val intent = Intent(this,MainActivity::class.java)
        val pi = PendingIntent.getActivity(this,0,intent,0)

//step3：创建一个消息

        val notification = NotificationCompat.Builder(this,"my_service")
            .setContentText("this is content title")
            .setSmallIcon(R.drawable.ic_launcher_background)
            .setContentIntent(pi)
            .build()

//step4：调用startForground启动前台service

        startForeground(1,notification)
    }

}
```

### 前台service的停止

stopForeground(false);

结束前台的服务，通知栏中该通知会随着点击或者滑动而删除。

参数是：是否删除之前发送的通知，true：删除。false：不删除 （用手滑动或者点击通知会被删除）



## IntentService
service的代码本质上还是运行在主线程中的，要在后台线程中运行service，需要在service的回调函数中增加线程启动。

而IntentService就是一个简易的用完即销毁的线程。

### 1、创建一个IntentService

```java
public class MyService extends IntentService {
    //这里必须有一个空参数的构造实现父类的构造,否则会报异常
    //java.lang.InstantiationException: java.lang.Class<***.MyService> has no zero argument constructor
    public MyService() {
        super("");
    }
    
    @Override
    public void onCreate() {
        System.out.println("onCreate");
        super.onCreate();
    }

    @Override
    public int onStartCommand(@Nullable Intent intent, int flags, int startId) {
        System.out.println("onStartCommand");
        return super.onStartCommand(intent, flags, startId);

    }

    @Override
    public void onStart(@Nullable Intent intent, int startId) {
        System.out.println("onStart");
        super.onStart(intent, startId);
    }

    @Override
    public void onDestroy() {
        System.out.println("onDestroy");
        super.onDestroy();
    }

    //这个是IntentService的核心方法,它是通过串行来处理任务的,也就是一个一个来处理
    @Override
    protected void onHandleIntent(@Nullable Intent intent) {
        System.out.println("工作线程是: "+Thread.currentThread().getName());
        String task = intent.getStringExtra("task");
        System.out.println("任务是 :"+task);
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
}
```
### 2、调用IntentService

在activity中用intent来实现startService

```
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        Intent intent = new Intent(this,MyService.class);
        intent.putExtra("task","播放音乐");
        startService(intent);
        intent.putExtra("task","播放视频");
        startService(intent);
        intent.putExtra("task","播放图片");
        startService(intent);
    }
}
```

### 3、原理：内部实现了一handlerThread