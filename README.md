
# Android Developer Guide
* [Android Developer Guide](#android-developer-guide)
* [一、语言基础](#一语言基础)
  * [1、java语言](#1java语言)
  * [2、Kotlin语言](#2kotlin语言)
  * [3、Flutter](#3flutter)
  * [4、其他前端语言和技术](#4其他前端语言和技术)
* [二、计算机基础知识](#二计算机基础知识)
  * [1、网络基础知识](#1网络基础知识)
    * [TCP协议](#tcp协议)
    * [UDP协议](#udp协议)
    * [Http协议](#http协议)
    * [Wifi](#wifi)
    * [Ble](#ble)
    * [MQTT](#mqtt)
  * [2、计算机硬件知识](#2计算机硬件知识)
  * [3、软件工程知识](#3软件工程知识)
    * [数据结构与算法](#数据结构与算法)
    * [设计模式](#设计模式)
    * [组件化、MVP、MVVM](#组件化mvpmvvm)
  * [源代码管理SVN和git](#源代码管理svn和git)
* [三、Android系统原理](#三android系统原理)
  * [Android系统框架](#android系统框架)
  * [Android系统启动过程](#android系统启动过程)
  * [Android 系统进程间通信机制](#android-系统进程间通信机制)
  * [Android App启动过程](#android-app启动过程)
  * [JNI与NDK](#jni与ndk)
* [四、Android基础开发知识](#四android基础开发知识)
  * [1、四大组件](#1四大组件)
  * [2、用户界面开发](#2用户界面开发)
      * [Material Design](#material-design)
      * [外观和风格](#外观和风格)
      * [布局layout](#布局layout)
    * [UI控件](#ui控件)
    * [动画](#动画)
    * [通知](#通知)
    * [多媒体](#多媒体)
    * [数据存储](#数据存储)
    * [网络编程（WebView）](#网络编程webview)
    * [线程](#线程)
    * [Jetpack](#jetpack)
* [四、Android发布相关](#四android发布相关)
  * [1、APK资源分析](#1apk资源分析)
  * [2、APK编译(打包)、安装、反编译、热修复](#2apk编译打包安装反编译热修复)
    * [编译(打包)过程](#编译打包过程)
    * [gradle](#gradle)
    * [混淆](#混淆)
    * [安装](#安装)
    * [反编译](#反编译)
    * [hook](#hook)
    * [热修复](#热修复)
    * [常见问题](#常见问题)
* [五、Android实践基础知识](#五android实践基础知识)
  * [1、Android兼容性](#1android兼容性)
  * [2、Android性能](#2android性能)
    * [启动优化](#启动优化)
    * [UI优化](#ui优化)
    * [存储优化](#存储优化)
    * [IO优化](#io优化)
    * [网络优化](#网络优化)
    * [耗电优化](#耗电优化)
    * [崩溃优化](#崩溃优化)
    * [内存优化](#内存优化)
    * [卡顿优化](#卡顿优化)
    * [安装包优化](#安装包优化)
    * [进程保活](#进程保活)
* [六、Android测试框架](#六android测试框架)
* [七、Android常用框架库原理](#七android常用框架库原理)
* [八、Android应用SDK](#八android应用sdk)
  * [支付](#支付)
  * [地图](#地图)
  * [推送](#推送)
  * [二维码](#二维码)
* [九、Android新技术](#九android新技术)
  * [1、AR](#1ar)

# 一、语言基础

## 1、java语言

* [java基础](https://github.com/geekist/developer_guide/blob/main/java/Java%20基础.md)

* [java虚拟机](https://github.com/geekist/developer_guide/blob/main/java/Java%20虚拟机.md)

* [javaIO](https://github.com/geekist/developer_guide/blob/main/java/Java%20IO.md)

* [java并发](https://github.com/geekist/developer_guide/blob/main/java/Java%20并发.md)

* [java容器](https://github.com/geekist/developer_guide/blob/main/java/Java%20容器.md)

## 2、Kotlin语言

* [基础](https://github.com/geekist/developer_guide/blob/main/kotlin/kotlin基础.md)

* [Kotlin标准函数库](https://github.com/geekist/developer_guide/blob/main/kotlin/standard.md)

* [类与对象](https://github.com/geekist/developer_guide/blob/main/kotlin/class.md)

* [函数与lambda表达式](https://github.com/geekist/developer_guide/blob/main/kotlin/function&lambda.md)

* [集合](https://github.com/geekist/developer_guide/blob/main/kotlin/kotlin集合.md)

* [Kotlin协程](https://github.com/geekist/developer_guide/blob/main/kotlin/coroutine.md)

* [Kotlin的常用语言结构](https://github.com/geekist/developer_guide/blob/main/kotlin/kotlin常用语言结构.md)

* [Kotlin中的5种单例模式](https://github.com/geekist/developer_guide/blob/main/kotlin/singlton.md)

## 3、Flutter

## 4、其他前端语言和技术

* [JavaScript](https://www.w3school.com.cn/js/index.asp)

* [Hybrid app](https://github.com/geekist/developer_guide/blob/main/other/hybrid.md)

* [React Native](https://github.com/geekist/developer_guide/blob/main/other/reactnative.md)

* [Flutter](https://github.com/geekist/developer_guide/blob/main/other/flutter.md)

* [Week](https://www.jianshu.com/p/ae1d7a2b0a8a) 

# 二、计算机基础知识

## 1、网络基础知识

### TCP协议

* [Tcp/IP协议簇概述](https://github.com/geekist/developer_guide/blob/main/network/tcp.md)


* [Android开发TCP](https://blog.csdn.net/VNanyesheshou/article/details/74896575)

* [tcpclient.java](https://github.com/geekist/developer_guide/blob/main/network/TCPClient.java)

* [tcpserver.java](https://github.com/geekist/developer_guide/blob/main/network/TCPServer.java)

### UDP协议

* [UDP协议简介](https://blog.csdn.net/yangwu007/article/details/109411342)

* [Android开发UDP](https://github.com/geekist/developer_guide/blob/main/network/udp_android.md)

* [UDPHub.java](https://github.com/geekist/developer_guide/blob/main/network/UdpHub.java)

### Http协议

* [Http概述](https://github.com/geekist/developer_guide/blob/main/network/Http.md)

### Wifi

* [Android Wifi开发总结](https://blog.csdn.net/qq_34773981/article/details/79163579)

### Ble

* [Android 蓝牙介绍](https://github.com/geekist/developer_guide/blob/main/bluetooth/ble介绍.md)

* [Android BLE 蓝牙开发入门](https://www.jianshu.com/p/3a372af38103)

* [Android Ble开发介绍2](https://www.jianshu.com/p/d991f0fdec63)

封装的几个蓝牙库

* [fastble](https://github.com/Jasonchenlijian/FastBle/wiki)

* [rxble](https://github.com/Belolme/RxBLE)


### MQTT

* [MQTT基本介绍](https://github.com/geekist/developer_guide/blob/main/IoT/MQTT.md)

* [阿里云IOT开发](https://github.com/geekist/developer_guide/blob/main/IoT/Android连接阿里云MQTT.md)

## 2、计算机硬件知识

* [硬件基础知识](https://github.com/geekist/developer_guide/blob/main/IoT/硬件基础知识.md)

* [Android CPU 架构详解](https://www.jianshu.com/p/44650eaceb18)

## 3、软件工程知识

### 数据结构与算法 

* [数据结构--略](https://github.com/geekist/developer_guide/blob/main/software/algrithom.md)

### 设计模式

* [设计模式](https://github.com/geekist/developer_guide/blob/main/designpatterns/patterns.md)

### 组件化、MVP、MVVM

* [Android组件化开发思想与实践](https://juejin.cn/post/6844904147641171981)

* [MVC，MVP，MVVM详细介绍](https://www.jianshu.com/p/ac5d4b2c2256)

* [android MVC、MVP and MVVM](https://www.jianshu.com/p/b9549aa0e1fe)


## 源代码管理SVN和git

* [git介绍](https://www.cnblogs.com/tugenhua0707/p/4050072.html)


* [git基本操作](https://www.runoob.com/git/git-basic-operations.html)

# 三、Android系统原理

## Android系统框架

* [Android系统框架](https://developer.android.com/guide/platform?hl=zh-cn)

## Android系统启动过程

* [Android系统启动流程](https://github.com/geekist/developer_guide/blob/main/android/system/android系统启动流程.md)

## Android 系统进程间通信机制

* [Android系统进程间通讯机制---Binder](https://github.com/geekist/developer_guide/blob/main/android/system/binder.md)

* [binder机制详细介绍](https://zhuanlan.zhihu.com/p/35519585)

## Android App启动过程

* [Android app 启动流程分析](https://github.com/geekist/developer_guide/blob/main/android/system/Android_App启动分析与优化.md)

## JNI与NDK

* [JNI编程](https://github.com/geekist/developer_guide/blob/main/android/system/jni.md)


# 四、Android基础开发知识

## 1、四大组件

* [Activity][activity] （[Fragment][fragment]）

* [BroadcastReceiver][broadcastReceiver]

* [ContentProvider][contentprovider]

* [Service][service]

* [Intent和Seriable以及Parcable](https://github.com/geekist/developer_guide/blob/main/android/4-components/Intent.md)

* [Context](https://www.jianshu.com/p/94e0f9ab3f1d)

[activity]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Activity.md
[fragment]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Fragment.md
[broadcastReceiver]:https://github.com/geekist/developer_guide/blob/main/android/4-components/BroadcastReceiver.md
[contentprovider]:https://github.com/geekist/developer_guide/blob/main/android/4-components/ContentProvider.md
[service]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Service.md

## 2、用户界面开发

#### Material Design

* [material design](https://github.com/geekist/developer_guide/blob/main/android/design/MaterialDesign.md)

#### 外观和风格
* [attrs、style和theme介绍](https://github.com/geekist/developer_guide/blob/main/android/ui/attrs.md)

#### 布局layout

* [LinearLayout][linearlayout]

* [RelativeLayout][relativelayout]

* [FrameLayout][framelayout]

* [ConstraintLayout][constraintlayout]

[linearlayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/LinearLayout.md

[relativelayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/RelativeLayout.md

[framelayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/FrameLayout.md

[constraintlayout]:https://github.com/geekist/developer_guide/blob/main/android/layout/ConstraintLayout.md

material design中新引入的布局layout

* [AppBarLayout][appbarlayout]

* [CoordinatorLayout][coordinatorlayout]

* [DrawerLayout][drawerlayout]

* [SwipeRefreshLayout][swiperefreshlayout]

* [第三开源库SmartRefreshLayout实现更多功能](https://github.com/scwang90/SmartRefreshLayout)

* [CollapsingToolbarLayout][collapsingtoolbarlayout]

### UI控件

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

**Material Desgin UI**

* [Toolbar][toolbar]

* [NavigationView][navigationview]--DrawerLayout+NavigationView实现侧滑栏

**TabLayout和ViewPager2**

* [TabLayout介绍](https://www.jianshu.com/p/88679fed9ecb)

* [TableLayout+ViewPager][table+viewPager]--多个Fragments在TableLayout下实现fragments横向滑动

* [TableLayout+ViewPager2][table+viewPager2]--多个Fragments在TableLayout下实现fragments横向滑动

* [第三方开源库FlycoTabLayout实现更加灵活的TabLayout功能](https://github.com/H07000223/FlycoTabLayout/blob/master/README_CN.md)

* [第三方开源库Banner实现轮播容器](https://github.com/youth5201314/banner)

**CardView**

* [MaterialCardView][cardview]

**RecyclerView**

* [RecyclerView][recyclerview]

**沉浸式状态栏**

* [沉浸式状态栏][transparentstatusbar]

* [第三方开源库ImmersionBar实现沉浸式状态栏](https://github.com/gyf-dev/ImmersionBar)

**Paging**

* [Paging](https://github.com/geekist/developer_guide/blob/main/android/ui/paging.md)

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

### 动画

* [Animation](https://github.com/geekist/developer_guide/blob/main/android/ui/Animation.md)

### 通知
* [Notification][notification]


[notification]:https://github.com/geekist/developer_guide/blob/main/android/notification/Notification.md

### 多媒体
* [图片][photo]

* [音频][audio]

* [视频][video]

[photo]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Photo.md
[audio]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Audio.md
[video]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Video.md

### 数据存储

* [SharedPreference][sharedpreference]


* [FileIO][fileio]


* [SQLite][sqlite]


[sharedpreference]:https://github.com/geekist/developer_guide/blob/main/android/database/SharedPreference.md
[fileio]:https://github.com/geekist/developer_guide/blob/main/android/database/FileIO.md
[sqlite]:https://github.com/geekist/developer_guide/blob/main/android/database/SQLite.md

### 网络编程（WebView）

* [webview](https://github.com/geekist/developer_guide/blob/main/android/ui/webview.md)

* [webview和Javascript交互](https://github.com/geekist/developer_guide/blob/main/android/ui/webview_h5.md)

* [第三方开源库agentweb封装webview实现带进度条的webview](https://github.com/Justson/AgentWeb)

### 线程

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

### Jetpack
* [Lifecycles][lifecycles]


* [ViewModel][viewmodel]


* [LiveData][livedata]


* [Room][room]


* [WorkManager][workmanager]

* [DataBinding](https://github.com/geekist/developer_guide/blob/main/android/jetpack/Databinding.md)

[lifecycles]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/Lifecycles.md

[viewmodel]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/ViewModel.md

[livedata]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/LiveData.md

[room]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/Room.md

[workmanager]:https://github.com/geekist/developer_guide/blob/main/android/jetpack/WorkManager.md

# 四、Android发布相关

## 1、APK资源分析

* [Android.manifest文件介绍]()

## 2、APK编译(打包)、安装、反编译、热修复

### 编译(打包)过程

* [Java程序编译和运行原理](https://github.com/geekist/developer_guide/blob/main/android/studio/java编译运行原理.md)

* [Android打包apk的流程](https://github.com/geekist/developer_guide/blob/main/android/studio/android打包apk流程.md) **[命令行记忆](https://www.cnblogs.com/sjm19910902/p/6416022.html)**

* [Android MultiDex原理及实现记录](https://www.jianshu.com/p/1c5e8f281d0d)

* [启用MultiDex](https://github.com/geekist/developer_guide/blob/main/android/studio/MultiDex.md)

### gradle

* [build.gradle文件介绍](https://github.com/geekist/developer_guide/blob/main/android/studio/Gradle.md)

* [深入理解gradle编译-Android基础篇](https://blog.csdn.net/chouhuan1877/article/details/100808667)

* [gradle实现Androidapp的编译过程](http://www.voidcn.com/article/p-meuukmnh-bch.html)

### 混淆

* [Android 代码混淆配置总结](https://www.cnblogs.com/renhui/p/9299786.html)

* [Android App代码混淆终极解决方案](http://www.androidchina.net/8367.html)

* [android 混淆的项目实例](https://github.com/geekist/developer_guide/blob/main/android/studio/混淆实例.md)

### 安装

* [Android安装apk的流程](https://github.com/geekist/developer_guide/blob/main/android/studio/android打包apk流程.md)

* [Android版本检测和安装](https://github.com/geekist/developer_guide/blob/main/android/studio/Update.md)


[qa]:https://github.com/geekist/developer_guide/blob/main/android/studio/QA.md

### 反编译

* [Android APK反编译讲解](https://www.cnblogs.com/geeksongs/p/10864200.html)

* [Android逆向破解工具Android Killer使用](https://www.jianshu.com/p/61a93a6c0c1b)

* [加固apk破解方式](https://blog.csdn.net/mr_jianrong/article/details/78958995)

### hook
 
* [Android Hook入门](https://www.jianshu.com/p/74c12164ffca?tdsourcetag=s_pcqq_aiomsg)

* [Android三大hook框架](https://www.dazhuanlan.com/2020/03/13/5e6a86cc0efb9/)

* [Android Hook技术防范漫谈](https://tech.meituan.com/2018/02/02/android-anti-hooking.html)

### 热修复

* [Android热修复学习指南](https://juejin.cn/post/6844903784330575879)

* [Andrid热修复技术回顾](https://segmentfault.com/a/1190000011365008)

### 常见问题

* [编译过程中常见问题][qa]
[qa]:https://github.com/geekist/developer_guide/blob/main/android/studio/QA.md


# 五、Android实践基础知识

##  1、Android兼容性

**Android版本兼容性**
* [Android平台版本兼容性问题总结](https://github.com/geekist/developer_guide/blob/main/android/compat/Android版本应用兼容性适配问题.md)

**Android不同屏幕兼容性**

* [Android屏幕适配解决方案](https://www.jianshu.com/p/ec5a1a30694b)

* [屏幕适配方案总结](https://juejin.cn/post/6844903599495970830)

* [AndroidAutoSize代码分析](https://blog.csdn.net/u012588160/article/details/105876735) && [Autosize使用](https://www.xiaoheidiannao.com/220402.html)

**Android不同厂商系统兼容性**

* [android不同厂商问题合集](https://www.zhihu.com/question/65594088/answer/232791937)


## 2、Android性能

### 启动优化

* [Android 启动分析与优化](https://github.com/geekist/developer_guide/blob/main/android/system/Android_App启动分析与优化.md)

参考：

* [启动优化分析-1](https://blog.yorek.xyz/android/paid/master/start_1/) 

* [启动优化分析-2](https://blog.yorek.xyz/android/paid/master/start_2/)
### UI优化
* []()

* []()

* []()

### 存储优化
* []()

* []()

* []()

### IO优化
* []()

* []()

* []()

### 网络优化
* []()

* []()

* []()

### 耗电优化

* [androird耗电分析与优化-1](https://blog.yorek.xyz/android/paid/master/battery_1/)

* [androird耗电分析与优化-2](https://blog.yorek.xyz/android/paid/master/battery_2/)

* [Android性能优化系列之电量优化](https://blog.csdn.net/u012124438/article/details/74617649)

* [深入探索 Android 电量优化](https://juejin.cn/post/6844904195523346439)


### 崩溃优化
* []()

* []()

* []()

### 内存优化

* [Android性能问题](https://github.com/geekist/developer_guide/blob/main/android/android性能调优.md)

### 卡顿优化
* []()

* []()

* []()

### 安装包优化
* []()

* []()

* []()

### 进程保活

* [Android进程保活](https://carsonho.blog.csdn.net/article/details/79522975)

# 六、Android测试框架

* [Android自动化测试框架介绍](https://github.com/geekist/developer_guide/blob/main/android/test/autotest.md)

* [Andoid单元测试介绍](https://github.com/geekist/developer_guide/blob/main/android/test/unittest.md)

# 七、Android常用框架库原理

* [ARouter][arouter]

* [EventBus][eventbus]

* [AutoSize][autosize]

* [OkGo][okgo]

* [Rxjava][rxjava]

* [Glide][glide]

* [Litepal][litepal]

[arouter]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ARouter.md
[eventbus]:https://github.com/geekist/developer_guide/blob/main/android/libraries/EventBus.md
[autosize]:https://github.com/geekist/developer_guide/blob/main/android/libraries/AndroidAutoSize.md
[okgo]:https://github.com/geekist/developer_guide/blob/main/android/libraries/OkGo.md
[rxjava]:https://github.com/geekist/developer_guide/blob/main/android/libraries/RxJava.md

[glide]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Glide.md

[litepal]:https://github.com/geekist/developer_guide/blob/main/android/libraries/LitePal.md

# 八、Android应用SDK


## 支付

* [微信和支付宝android客户端接入流程](https://github.com/geekist/developer_guide/blob/main/pay/alipay.md)

## 地图

## 推送

## 二维码


# 九、Android新技术 

## 1、AR

* [关于AR的基本概念和现状](https://blog.csdn.net/qq_32138419/article/details/106850796)

* [ARCore学习之旅：基础概念](https://juejin.cn/post/6844903493900173319)

----------

[老版本入口](https://github.com/geekist/developer_guide/blob/main/README2.md)



