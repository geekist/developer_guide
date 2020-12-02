
# Android.manifest文件介绍

* tools:context="com.android.example.MainActivity"
context属性就是称呼的Activity属性，有助于ide加载布局，他可以在Android studio代码中找到相关文件

* tools:menu
告诉IDE 在预览窗口中使用哪个菜单，这个菜单将显示在layout的根节点上（actionbar的位置）。

* tools:ignore
ignore属性是告诉Lint忽略xml中的某些警告。


tools:ignore="AllowBackup,RtlEnabled"

Android API Level 8及其以上Android系统提供了为应用程序数据的备份和恢复功能，此功能的开关决定于该应用程序中AndroidManifest.xml文件中的allowBackup属性值[1] ，其属性值默认是True。当allowBackup标志为true时，用户即可通过adb backup和adb restore来进行对应用数据的备份和恢复，这可能会带来一定的安全风险。

Android属性allowBackup安全风险源于adb backup容许任何一个能够打开USB 调试开关的人从Android手机中复制应用数据到外设，一旦应用数据被备份之后，所有应用数据都可被用户读取；adb restore容许用户指定一个恢复的数据来源（即备份的应用数据）来恢复应用程序数据的创建。因此，当一个应用数据被备份之后，用户即可在其他Android手机或模拟器上安装同一个应用，以及通过恢复该备份的应用数据到该设备上，在该设备上打开该应用即可恢复到被备份的应用程序的状态。

尤其是通讯录应用，一旦应用程序支持备份和恢复功能，攻击者即可通过adb backup和adb restore进行恢复新安装的同一个应用来查看聊天记录等信息；对于支付金融类应用，攻击者可通过此来进行恶意支付、盗取存款等；因此为了安全起见，开发者务必将allowBackup标志值设置为false来关闭应用程序的备份和恢复功能，以免造成信息泄露和财产损失。

RtlEnabled表示支持从右向左的方式。

* tools:targetApi="LOLLIPOP"
假设minSdkLevel 15，而你使用了api21中的控件比如RippleDrawable
使用这个可以不显示这个警告

* tools:local="it"
tools:locale（本地语言）属性
默认情况下res/values/strings.xml中的字符串会执行拼写检查，如果不是英语，会提示拼写错误，通过以下代码来告诉studio本地语言不是英语，就不会有提示了。


```xml
    <activity
        android:name=".ui.activity.MainActivity"
        android:configChanges="orientation|keyboardHidden|screenSize"

使用android:configChanges属性后，程序运行时如果配置发生变化时，不会重新启动activity而是通知程序去调用onConfigurationChanged函数。例如：在横竖屏发生变化时，原来会重新启动activity，而定义了这个属性后，就不会重新启动activity了，而是调用onConfigrationChanged函数


        android:hardwareAccelerated="true"

 android:hardwareAccelerated="true"， 会使对于图片类比较多的界面加载速度变快。但是小心该选项设置带来的内存增加。 该选项是以牺牲内存来提高响应速度的。 


        android:screenOrientation="portrait"  //只显示竖屏


        android:theme="@style/AppTheme.Launcher"

        android:windowSoftInputMode="adjustResize"

>
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <action android:name="android.intent.action.VIEW" />  // 浏览器

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
    </activity>

```

android:windowSoftInputMode="adjustResize"
1、"stateUnspecified"

软键盘的状态 (是否它是隐藏或可见 )没有被指定。系统将选择一个合适的状态或依赖于主题的设置。

这个是为了软件盘行为默认的设置。

 

2、"stateUnchanged"

软键盘被保持无论它上次是什么状态，是否可见或隐藏，当主窗口出现在前面时。

 

3、"stateHidden"

当用户选择该 Activity时，软键盘被隐藏——也就是，当用户确定导航到该 Activity时，而不是返回到它由于离开另一个 Activity。

 

4、"stateAlwaysHidden"

软键盘总是被隐藏的，当该 Activity主窗口获取焦点时。

 

5、"stateVisible"

软键盘是可见的，当那个是正常合适的时 (当用户导航到 Activity主窗口时 )。

 

6、"stateAlwaysVisible"

当用户选择这个 Activity时，软键盘是可见的——也就是，也就是，当用户确定导航到该 Activity时，而不是返回到它由于离开另一个 Activity。

 

7、"adjustUnspecified"

它不被指定是否该 Activity主 窗口调整大小以便留出软键盘的空间，或是否窗口上的内容得到屏幕上当前的焦点是可见的。系统将自动选择这些模式中一种主要依赖于是否窗口的内容有任何布局 视图能够滚动他们的内容。如果有这样的一个视图，这个窗口将调整大小，这样的假设可以使滚动窗口的内容在一个较小的区域中可见的。这个是主窗口默认的行为 设置。

 

8、"adjustResize"

该 Activity主窗口总是被调整屏幕的大小以便留出软键盘的空间

 

9、"adjustPan"

该 Activity主窗口并不调整屏幕的大小以便留出软键盘的空间。相反，当前窗口的内容将自动移动以便当前焦点从不被键盘覆盖和用户能总是看到输入内容的部分。这个通常是不期望比调整大小，因为用户可能关闭软键盘以便获得与被覆盖内容的交互操作。


## 从网页打开app
用户在访问我们的网页时，判断出这个用户手机上是否安装了我们的App，如果安装了则直接从网页上打开APP，否则就引导用户前往下载，从而形成一个推广上的闭环。这里只针对从网页端打开本地APP。
```java
<activity
    android:name=".ui.activity.ZMCertTestActivity"
    android:label="@string/app_name"
    android:launchMode="singleTask"
    android:screenOrientation="portrait">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:scheme="scheme1"
            android:host="host1"
            android:path="/path1"
            android:port="8080" />
    </intent-filter>
</activity>

```

WEB端通过调用“scheme1://host1:8080/path1?query1=1&query2=true“便能打开这个Activity。其中scheme和host是必须的，另外的看需求。



