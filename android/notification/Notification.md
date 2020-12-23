

# Notification
当应用程序希望向用户发送一些提示信息，而用户又不在前台时，可以借助通知来实现。

## 通知渠道
Android8.0以后提出了通知渠道的概念，方便用户选择接受那个渠道的通知。
```java
val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
   if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        val channel = NotificationChannel("notify","Notify",NotificationManager.IMPORTANCE_HIGH)
        notificationManager.createNotificationChannel(channel)
    }
```
## 创建通知
```java
  val notification = NotificationCompat.Builder(this,"notify")
                .setContentTitle("Important Notification")
                .setContentText("hello, i am fine, this is my notification")
                .setSmallIcon(R.drawable.ic_launcher_foreground)
                .setLargeIcon(BitmapFactory.decodeResource(resources,R.drawable.ic_launcher_foreground))
                .build()


```

## 发送通知
```java
   notificationManager.notify(1,notification) //notification Id, notification
```

## 点击事件 ---PendingIntent
```java
        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel("notify","Notify",NotificationManager.IMPORTANCE_HIGH)
            notificationManager.createNotificationChannel(channel)
        }

        val intent = Intent(this,NotificationActivity::class.java)
        val pi = PendingIntent.getActivity(this,0,intent,0)

        val notification = NotificationCompat.Builder(this,"notify")
                .setContentTitle("Important Notification")
                .setContentText("hello, i am fine, this is my notification")
                .setSmallIcon(R.drawable.ic_launcher_foreground)
                .setLargeIcon(BitmapFactory.decodeResource(resources,R.drawable.ic_launcher_foreground))

                .setContentIntent(pi)
                //.setAutoCancel(true)

                .build()
        notificationManager.notify(1,notification)
```
## 取消通知在界面的显示
可以在创建通知的时候设置
setAutoCancel（）

或者在新的页面创建的时候调用 
notificationManger.cancel(id)

## 复杂界面通知
```java
        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel("notify","Notify",NotificationManager.IMPORTANCE_HIGH)
            notificationManager.createNotificationChannel(channel)
        }

        val intent = Intent(this,NotificationActivity::class.java)
        val pi = PendingIntent.getActivity(this,0,intent,0)

        val notification = NotificationCompat.Builder(this,"notify")
                .setContentTitle("Important Notification")
                .setContentText("hello, i am fine, this is my notification")
                .setSmallIcon(R.drawable.ic_launcher_foreground)
                .setLargeIcon(BitmapFactory.decodeResource(resources,R.drawable.ic_launcher_foreground))

                .setContentIntent(pi)

.setStyle(NotificationCompat.BigTextSytle().bigText())
.setSytle(NotificationCompat.BigPictureSytle().bigPicture())



               
                .build()
        notificationManager.notify(1,notification)
```
