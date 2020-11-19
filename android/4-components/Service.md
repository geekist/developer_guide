

# Service

## Service定义
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

    override fun onBind(intent: Intent): IBinder {
        return mBinder
        // Log.d(TAG,"on Bind")
        // return null
    }

    override fun onCreate() {
        super.onCreate()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Log.d(TAG, "on start")
        return super.onStartCommand(intent, flags, startId)
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d(TAG, "on destroy")

    }
}

```
同时，manifest文件中要注册这个service
```xml
  <service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>
```

## Service的启动和停止
```kotlin
 val intent = Intent(this,MyService::class.java)
            startService(intent)

 val intent = Intent(this,MyService::class.java)
            stopService(intent)
```

## Activity和Service的binding

```kotlin
  buttonBindService.setOnClickListener{
            val intent = Intent(this,MyService::class.java)
            bindService(intent,connection, Context.BIND_AUTO_CREATE)
        }

        buttonUnBindService.setOnClickListener{
            unbindService(connection)
        }

```
然后通过connection得到Binder的实例，通过实例来控制service中的某些过程

## Service的生命周期

## 前台service
在service的onCreate函数中，添加一个通知，然后start
```kotlin
override fun onCreate() {
        super.onCreate()
        Log.d(TAG, "on create")

        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel(
                "my_service",
                "qiantai service",
                NotificationManager.IMPORTANCE_DEFAULT
            )
            manager.createNotificationChannel(channel)
        }

        val intent = Intent(this,MainActivity::class.java)
        val pi = PendingIntent.getActivity(this,0,intent,0)
        val notification = NotificationCompat.Builder(this,"my_service")
            .setContentText("this is content title")
            .setSmallIcon(R.drawable.ic_launcher_background)
            .setContentIntent(pi)
            .build()
        startForeground(1,notification)
    }
```

## IntentService
service的代码本质上还是运行在主线程中的，要在后台线程中运行service，需要在service的回调函数中增加线程启动。

而IntentService就是一个简易的用完即销毁的线程。

```java
class MyIntentService : IntentService("MyIntentService") {

    override fun onHandleIntent(intent: Intent?) {
       Log.d("","sfasfsfsfssffsafsfsfsfsfsfsf")
    }

    override fun onDestroy() {
        super.onDestroy()
        Log.d("","fsafsfsfsfsfasfasfasfasfasfasf")

    }
}

   val intent = Intent(this,MyIntentService::class.java)
            startService(intent)
```