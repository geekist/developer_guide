# Activity

需要掌握：

**1、Activity概念**

**2、创建一个Fragment**

**3、启动和销毁activity**

**4、在Activity之间传递数据**

**5、Activity的启动模式**

**5、Activity的声明周期**

## Activity概念

Activity是一个应用程序组件，它提供一个屏幕作为用户与Android应用程序交互的接口，通过这个组件可以放置各种控件，通过setContentView(View)来显示指定控件，可以监听并处理用户的事件做出响应。Activity之间通过Intent进行通信。

从设计层面上来讲，activity是Mvc设计模式中的Controller控制层，在Android中，通过Activity选择要显示的View,从View中获取数据然后传给Model层进行处理，最后显示出来。

## 创建一个Activity

* 1-在AndroidStudio中创建一个NoActivity的工程，添加一个Activity，命名位FirstActivity。
```java

class FirstActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
    }
}

```

系统会为Activity添加布局文件，新建一个layout文件，命名为first_layout
```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">
    
    <Button
        android:id="@+id/button_1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="button1">
    </Button>

</LinearLayout>

```
将布局文件和Activity绑定起来
```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.first_layout);
    }

```
* 2-在Android.manifest文件中注册Activity.可以看到系统已经为我们注册好了，我们需要添加一个Intent filter，告诉系统如何调用。
```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.jon.chapter301">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.Chapter301">
        <activity android:name=".FirstActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
现在，一个新的Activity就可以启动了

* Activity中控件的自动绑定

在app的build.gradle中加入：kotlin-android-extensions的插件即可
```java
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
}

```

## 启动和销毁Activity

**通过Intent，调用startActivity或startActivityForResult启动activity**

是Android程序中各个组件之间进行交互的一种重要方式，它不仅可以指出当前组件想要执行的动作，也可以在不同组件之间传递数据。Intent一般可以用来启动Activity、Service、以及发送广播消息等。


Intent分为两种：显示Intent和隐式Intent
* 显示Intent

有一种简单的定义Intent的方式用来启动一个Activity
Intent(Context context,Activity::class.java)

* 隐式Intent

隐式Intent不明确指出需要调用那一个Activity，而是指定一系列更为抽象的action和category信息然后交由系统来分析，并由系统找出合适的Activity去启动。
```java
//指定特定的intent的action和category的activity
val Intent = Intent("com.example.activitytest.ACTION_START")
intent.addCategory("com.example.activitytest.MY_CATEGORY")
startActivity(intent)
```

//这里需要注意，category 一定要加上
```java
 <activity android:name=".SecondActivity">
            <intent-filter>
                <action android:name="com.jon.chapter3011.ACTION_HELLO"> </action>
                <category android:name="android.intent.category.DEFAULT" />

            </intent-filter>
```
更多的隐式调用的用法：
```java
 intent = Intent(Intent.ACTION_VIEW)
            intent.data = Uri.parse("https://www.baidu.com")
            startActivity(intent)

这里使用了data标签，data标签还可以配置：

Android：scheme：数据的协议：比如：htttps

Android：host 指定主机名部分。

Android：port指定端口

Android：path指定端口后面的部分

Android：mimeType指定数据类型。
```

```java

intent = Intent(Intent.ACTION_DIAL)
            intent.data = Uri.parse("tel:10086")
            startActivity(intent)

```

**Activity的销毁**

* activity.finish（）

* 如果需要将应用程序彻底清除，则需要调用killProcess或者system.exit：

```java

System.exit(0)；

Process.killProcess(Process.myPid())；

```


##在Activity之间传递数据 

**1-传递数据给下一个activity**

使用Intent的putExtra函数可以给下一个activity传递数据，然后在下一个Activity的OnCreate中可以接受数据

```java
intent.putExtra("data_test","hello,world.....")
startActivity(intent)
```

**2-从上一个activity接受数据**

Activity的OnCreate中可以接受数据
```java
val msg = intent.getStringExtra("data_test")
setTitle(msg)

```

**3-从下一个activity返回后带回来的数据**

* 启动activity，并指明要返回的数据

要使用startActivityForResult来启动下一个Activity，函数中带有一个request_code
```java
val Intent = Intent("com.example.activitytest.ACTION_START")
intent.addCategory("com.example.activitytest.MY_CATEGORY")
startActivityForResult(intent)

startActivityForResult(new Intent(MainActivity.this, OtherActivity.class), 1);

```
* 被启动的activity添加数据,在关闭Activity前使用setResult函数
```java
  val intent = Intent()
  intent.putExtra("return_data","I am back")
  setResult(RESULT_OK,intent)
  finish()
```

* 在启动activity的onActivityResult中得到数据

```java
  override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
      if(resultCode == RESULT_OK) {
          when(requestCode) {
              1->{
                  val str = data?.getStringExtra("return_data")
                  setTitle(str)
              }
          }
        }
        super.onActivityResult(requestCode, resultCode, data)
    }

```

**4-Activity被回收时保存数据，等待再次创建时得到**

如果activity被系统回收，则可以通过onSaveInstanceState来保存一些临时数据，
然后在activity的onCreate中得到这些数据
```java
 override fun onSaveInstanceState(outState: Bundle, outPersistentState: PersistableBundle) {
        
        outState.putString("data_save","I nedd save")
        
        super.onSaveInstanceState(outState, outPersistentState)
    }

```
在onCreate中得到这些数据

```java
override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.first_layout)
        
        if(savedInstanceState != null) {
            val tempData = savedInstanceState.getString("data_save")
        }
}

```

## Activity的启动模式
* standard 标准启动模式，不管activity栈中是否有实例存在，都创建一个新的实例

* singleTop  如果栈顶存在activity，则不创建新的实例，改用该实例

* singleTask  如果堆栈中存在activity，则推到栈顶，在它之上的activity被销毁

* singleInstance 创建一个新的activity 栈，只存放这一个activity


## Activity的生命周期

* onCreate

* onStart

* onResume

* onPause

* onStop

* onDestroy

* onRestart

除了最后一个回调函数外，其他都是两两对应的。

onCreate和onDestroy中间是一个完整的生存周期
onStart和onStop是一个可见周期
onResume和onPause是一个存活周期
onRestart是从onStop之后被从堆栈中唤醒。


## 实践技巧






