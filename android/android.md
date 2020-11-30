
# Android

## Android Studio

### 编译和运行
* [build.gradle介绍][gradle]
* [常见问题][qa]

[gradle]:https://github.com/geekist/developer_guide/blob/main/android/studio/Gradle.md

### 调试和测试

* [启用MultiDex][multidex]


### 发布
[qa]:https://github.com/geekist/developer_guide/blob/main/android/studio/QA.md

## Android开发

### 四大组件

* [Activity][activity] （[Fragment][fragment]）
* [BroadcastReceiver][broadcastReceiver]
* [ContentProvider][contentprovider]
* [Service][service]

[activity]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Activity.md
[fragment]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Fragment.md
[broadcastReceiver]:https://github.com/geekist/developer_guide/blob/main/android/4-components/BroadcastReceiver.md
[contentprovider]:https://github.com/geekist/developer_guide/blob/main/android/4-components/ContentProvider.md
[service]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Service.md

### UI 控件

* [控件常用的属性][common]
* [TextView][textview]
* [Button][button]
* [EditText][edittext]
* [ImageView][imageview]
* [ProgressBar][progressbar]
* [AlertDialog][alertdialog]
* [ListView][listview]
* [Toast][toast]
* [Menu][menu]
* [自定义控件][self]

[common]:https://github.com/geekist/developer_guide/blob/main/android/ui/Common.md
[toast]:https://github.com/geekist/developer_guide/blob/main/android/ui/Toast.md
[menu]:https://github.com/geekist/developer_guide/blob/main/android/ui/Menu.md
[textview]:https://github.com/geekist/developer_guide/blob/main/android/ui/TextView.md
[button]:https://github.com/geekist/developer_guide/blob/main/android/ui/Button.md
[edittext]:https://github.com/geekist/developer_guide/blob/main/android/ui/EditText.md
[imageview]:https://github.com/geekist/developer_guide/blob/main/android/ui/ImageView.md
[progressbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/ProgressBar.md
[alertdialog]:https://github.com/geekist/developer_guide/blob/main/android/ui/AlertDialog.md

[listview]:https://github.com/geekist/developer_guide/blob/main/android/ui/ListView.md

[self]:https://github.com/geekist/developer_guide/blob/main/android/ui/自定义控件.md



### Mfaterial Design UI
* 依赖项的引入
在app的build.gradle中加入下面的依赖项
```java
dependencies {
    implementation 'com.google.android.material:material:1.2.1'
}
```

* 命名空间
注意引入material Design后，布局文件中增加了命名空间：
```xml
 xmlns:android="http://schemas.android.com/apk/res/android"
 xmlns:app="http://schemas.android.com/apk/res-auto"
 xmlns:tools="http://schemas.android.com/tools"
```
引入app命名空间，是因为许多MaterialDesign的属性是新增加的，原来的android命名空间中并不存在，所以为了兼容老系统，增加了一个新的命名空间。

* [AppBarLayout][appbarlayout]
* [CoordinatorLayout][coordinatorlayout]
* [DrawerLayout][drawerlayout]
* [SwipeRefreshLayout][swiperefreshlayout]
* [CollapsingToolbarLayout][collapsingtoolbarlayout]


* [Toolbar][toolbar]
* [NavigationView][navigationview]--DrawerLayout+NavigationView实现侧滑栏
* [TableLayout+ViewPager][table+viewPager]--多个Fragments在TableLayout下实现fragments横向滑动
* [TableLayout+ViewPager2][table+viewPager2]--多个Fragments在TableLayout下实现fragments横向滑动
* [MaterialCardView][cardview]
* [RecyclerView][recyclerview]
* [沉浸式状态栏][transparentstatusbar]



[navigationview]:https://github.com/geekist/developer_guide/blob/main/android/ui/NavigationView.md
[appbarlayout]:https://github.com/geekist/developer_guide/blob/main/android/ui/AppBarLayout.md

[drawerlayout]:https://github.com/geekist/developer_guide/blob/main/android/ui/DrawerLayout.md

[coordinatorlayout]:https://github.com/geekist/developer_guide/blob/main/android/ui/CoordinatorLayout.md
[swiperefreshlayout]:https://github.com/geekist/developer_guide/blob/main/android/ui/SwipeRefreshLayout.md
[collapsingtoolbarlayout]:https://github.com/geekist/developer_guide/blob/main/android/ui/CollapsingToolbarLayout.md

[toolbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/Toolbar.md
[cardview]:https://github.com/geekist/developer_guide/blob/main/android/ui/MaterialCardView.md
[transparentstatusbar]:https://github.com/geekist/developer_guide/blob/main/android/ui/StatusBar.md
[recyclerview]:https://github.com/geekist/developer_guide/blob/main/android/ui/RecyclerView.md
[table+viewPager]:https://github.com/geekist/developer_guide/blob/main/android/ui/Activity+Fragments+TableLayout+ViewPager.md
[table+viewPager2]:https://github.com/geekist/developer_guide/blob/main/android/ui/Activity+Fragments+TableLayout+ViewPager2.md


### 通知
* [通知][notification]


[notification]:https://github.com/geekist/developer_guide/blob/main/android/notification/Notification.md

### 多媒体
* [图片][photo]
* [音频][audio]
* [视频][video]

[photo]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Photo.md
[audio]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Audio.md
[video]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Video.md

### [Layout 布局文件][layout]

* [LinearLayout][linearlayout]
* [RelativeLayout][relativelayout]
* [FrameLayout][framelayout]
* [ConstraintLayout][constraintlayout]


[linearlayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/LinearLayout.md

[relativelayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/RelativeLayout.md

[framelayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/FrameLayout.md

[constraintlayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/ConstraintLayout.md

### [资源文件Resource Files][resource]

## Android数据存储
* [SharedPreference][sharedpreference]
* [FileIO][fileio]
* [SQLite][sqlite]


[sharedpreference]:https://github.com/geekist/developer_guide/blob/main/android/database/SharedPreference.md
[fileio]:https://github.com/geekist/developer_guide/blob/main/android/database/FileIO.md
[sqlite]:https://github.com/geekist/developer_guide/blob/main/android/database/SQLite.md

## Intent
* [Intent][intent]

[intent]:https://github.com/geekist/developer_guide/blob/main/android/intent/Intent.md

## 线程
* [Java线程][javathread]
* [Handler][handler]
* [HandlerThread][handlerthread]
* [AsyncTask][asynctask]
* [IntentService][intentservice]

[javathread]:https://github.com/geekist/developer_guide/blob/main/android/thread/JavaThread.md
[handler]:https://github.com/geekist/developer_guide/blob/main/android/thread/Handler.md
[handlerthread]:https://github.com/geekist/developer_guide/blob/main/android/thread/HandlerThread.md
[asynctask]:https://github.com/geekist/developer_guide/blob/main/android/thread/AsyncTask.md
[intentservice]:https://github.com/geekist/developer_guide/blob/main/android/thread/IntentService.md

## Jetpack
* [Lifecycles][lifecycles]
* [ViewModel][viewmodel]
* [LiveData][livedata]
* [Room][room]
* [WorkManager][workmanager]

[lifecycles]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/Lifecycles.md

[viewmodel]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/ViewModel.md

[livedata]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/LiveData.md

[room]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/Room.md

[workmanager]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/WorkManager.md


## Android常用的库libraries

* [ARouter][arouter]

* [EventBus][eventbus]

* [AutoSize][autosize]

* [OkGo][okgo]

* [Rxjava][rxjava]

* [Glide][glide]

* [Litepal][litepal]


- retrofit

- gson

- pgyersdk

- pgyersdk

- dns



- sweet alert dialog


- 科大讯飞语音集成



- 个推 消息推送


#### 2、权限申请

#### 3、通用部分

#### 4、硬件通讯

#### 5、服务器通信




[arouter]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ARouter.md
[eventbus]:https://github.com/geekist/developer_guide/blob/main/android/libraries/EventBus.md
[autosize]:https://github.com/geekist/developer_guide/blob/main/android/libraries/AndroidAutoSize.md
[okgo]:https://github.com/geekist/developer_guide/blob/main/android/libraries/OkGo.md
[rxjava]:https://github.com/geekist/developer_guide/blob/main/android/libraries/RxJava.md

[glide]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Glide.md

[litepal]:https://github.com/geekist/developer_guide/blob/main/android/libraries/LitePal.md












[ui]:https://github.com/geekist/developer_guide/blob/main/android/ui/ui.md
[layout]:https://github.com/geekist/developer_guide/blob/main/android/layout/Layout.md

[resource]:https://github.com/geekist/developer_guide/blob/main/android/layout/resource.md

[libraries]:https://github.com/geekist/developer_guide/blob/main/android/libraries/libraries.md

[multidex]:https://github.com/geekist/developer_guide/blob/main/android/studio/MultiDex.md