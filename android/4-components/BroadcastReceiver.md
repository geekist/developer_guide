

# BroadcastReceiver

## Broadcast分类
### 按照broadcast的类型来分，可以分为标准广播和有序广播
* staandard broadcast  
标准广播是一种完全异步执行的广播，发出后所有注册了该广播的接受者几乎会在同一时间内接收到该广播消息，这个消息没有先后顺序，不可以被阻截。  

* ordered broadcast   
有序广播是一种同步执行的广播，广播发出后，同一时刻只有一个接受者能够收到这条广播消息。接受者根据优先级来判定。当该广播消息被处理完成后广播才会继续向下传递，而且接受者可以截断广播，这样后面的接受者就无法接受广播消息了。

### 按照发送者来分，可以分为系统广播和自定义广播

## 发送一个Broadcast ---- 通过Intent
```kotlin
val intent = Intent("com.example.broadcastTest.MY_BROADCAST") //消息名
intent.setPackage(packageName)  //显示指定消息的接受对象
sendBroadcast(intent)
```

```kotlin
val intent = Intent("com.example.broadcastTest.MY_BROADCAST") //消息名
intent.setPackage(packageName)  //显示指定消息的接受对象
sendOrderedBroadcast(intent,null)
```

## 注册BroadcastReceiver来接受系统广播的两种方法

### 静态注册
* step1:首先创建一个BroadcastReceiver的子类，实现BroadcastReceiver的onReceive功能

```kotlin
class BootCompleteReceiver: BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        Toast.makeText(context,
            "cur Thread is: " + Thread.currentThread().name,
            Toast.LENGTH_LONG)
            .show()
    }
}
```
* step2: 在Android.manifest中注册receiver，指明接受广播的类型
```kotlin

        <receiver android:name=".BootCompleteReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
               ` <action android:name="android.intent.action.BOOT_COMPLETED">`
                </action>
            </intent-filter>
```

* step3: 用静态方式注册的receiver，只能接受一部分的系统消息，且需要在manifest中申请权限
```kotlin
    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED">
```
注：上述的例子在小米手机没有测试成功

### 动态注册 
* step1：创建一个BroadcastReceiver的子类，实现broadcast的方法。
* step2: 用代码创建一个Intent来注册receiver 
```kotlin
package com.jon.chapter0601

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log

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
