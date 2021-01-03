
# Android App 启动分析与优化

## app的启动方式：

1.）冷启动
 当启动应用时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用，这个启动方式就是冷启动。冷启动因为系统会重新创建一个新的进程分配给它，所以会先创建和初始化Application类，再创建和初始化MainActivity类（包括一系列的测量、布局、绘制），最后显示在界面上。

2.）热启动
当启动应用时，后台已有该应用的进程（例：按back键、home键，应用虽然会退出，但是该应用的进程是依然会保留在后台，可进入任务列表查看），所以在已有进程的情况下，这种启动会从已有的进程中来启动应用，这个方式叫热启动。热启动因为会从已有的进程中来启动，所以热启动就不会走Application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化Application，因为一个应用从新进程的创建到进程的销毁，Application只会初始化一次。

## app的启动流程：

通过上面的两种启动方式可以看出app启动流程为：

Application的构造器方法——>attachBaseContext()——>onCreate()——>Activity的构造方法——>onCreate()——>配置主题中背景等属性——>onStart()——>onResume()——>测量布局绘制显示在界面上

## app的启动优化：

基于上面的启动流程我们尽量做到如下几点

* Application的创建过程中尽量少的进行耗时操作

* 如果用到SharePreference,尽量在异步线程中操作

* 减少布局的层次,并且生命周期回调的方法中尽量减少耗时的操作


## app启动遇见黑屏或者白屏问题

1.）产生原因
其实显示黑屏或者白屏实属正常，这是因为还没加载到布局文件，就已经显示了window窗口背景，黑屏白屏就是window窗口背景。

2.）解决办法
通过设置设置Style

（1）设置背景图Theme

通过设置一张背景图。 当程序启动时，首先显示这张背景图，避免出现黑屏
```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:screenOrientation">portrait</item>
        <item name="android:windowBackground">>@mipmap/splash</item>
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:windowNoTitle">true</item>
</style>
```
（2）设置透明Theme

通过把样式设置为透明，程序启动后不会黑屏而是整个透明了，等到界面初始化完才一次性显示出来
```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowBackground">@android:color/transparent</item>
        <item name="android:windowIsTranslucent">true</item>
        <item name="android:screenOrientation">portrait</item>
    </style>
```
两者对比：

Theme1 程序启动快，界面先显示背景图，然后再刷新其他界面控件。给人刷新不同步感觉。
Theme2 给人程序启动慢感觉，界面一次性刷出来，刷新同步。
（3）修改AndroidManifest.xml
```xml
<application
        android:name=".App"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true">
        <activity android:name=".MainActivity"
         android:theme="@style/AppTheme">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
 
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
 
    //......
 
</application>
```

3.）常见的Theme主题
```xml
android:theme="@android:style/Theme.Dialog" //Activity显示为对话框模式
 
android:theme="@android:style/Theme.NoTitleBar" //不显示应用程序标题栏
 
android:theme="@android:style/Theme.NoTitleBar.Fullscreen" //不显示应用程序标题栏，并全屏
 
android:theme="Theme.Light " //背景为白色
 
android:theme="Theme.Light.NoTitleBar" //白色背景并无标题栏
 
android:theme="Theme.Light.NoTitleBar.Fullscreen" //白色背景，无标题栏，全屏
 
android:theme="Theme.Black" //背景黑色
 
android:theme="Theme.Black.NoTitleBar" //黑色背景并无标题栏
 
android:theme="Theme.Black.NoTitleBar.Fullscreen" //黑色背景，无标题栏，全屏
 
android:theme="Theme.Wallpaper" //用系统桌面为应用程序背景
 
android:theme="Theme.Wallpaper.NoTitleBar" //用系统桌面为应用程序背景，且无标题栏
 
android:theme="Theme.Wallpaper.NoTitleBar.Fullscreen" //用系统桌面为应用程序背景，无标题栏，全屏
 
android:theme="Theme.Translucent" //透明背景
 
android:theme="Theme.Translucent.NoTitleBar" //透明背景并无标题
 
android:theme="Theme.Translucent.NoTitleBar.Fullscreen" //透明背景并无标题，全屏
 
android:theme="Theme.Panel " //面板风格显示
 
android:theme="Theme.Light.Panel" //平板风格显示
```