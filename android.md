
目录：

- [一、Android系统原理](#一android系统原理)
  - [Android的CPU架构](#android的cpu架构)
  - [Android系统框架和启动过程](#android系统框架和启动过程)
  - [Android 进程间通信机制](#android-进程间通信机制)
  - [JNI与NDK](#jni与ndk)
  - [Android版本迭代](#android版本迭代)
  - [Android厂商兼容性问题](#android厂商兼容性问题)
- [二、Android Framework 基础知识](#二android-framework-基础知识)
  - [1、四大组件](#1四大组件)
    - [1.1 四大组件的简介和启动过程](#11-四大组件的简介和启动过程)
  - [2、View](#2view)
    - [2.1 android UI 控件的介绍基本用法](#21-android-ui-控件的介绍基本用法)
    - [2.2 window和view](#22-window和view)
    - [2.3 android对话框](#23-android对话框)
    - [3、动画](#3动画)
  - [4、通知](#4通知)
  - [5、多媒体](#5多媒体)
  - [6、数据存储](#6数据存储)
  - [7、网络编程（WebView）](#7网络编程webview)
  - [8、Android消息机制与Android线程和线程池](#8android消息机制与android线程和线程池)
    - [8.1 线程和线程池](#81-线程和线程池)
    - [8.2消息机制--Handler、Looper，Message、MessageQueue](#82消息机制--handlerloopermessagemessagequeue)
    - [8.3 Android的异步调用方法](#83-android的异步调用方法)
  - [9、Jetpack](#9jetpack)
- [三、Android性能调优](#三android性能调优)
  - [1、启动优化](#1启动优化)
  - [2、UI优化](#2ui优化)
  - [3、ANR与崩溃定位与分析](#3anr与崩溃定位与分析)
  - [4、内存优化](#4内存优化)
  - [5、卡顿优化](#5卡顿优化)
  - [6、耗电优化](#6耗电优化)
  - [7、存储优化](#7存储优化)
  - [8、进程保活](#8进程保活)
- [四、Android测试框架](#四android测试框架)
- [五、Android应用编译和发布](#五android应用编译和发布)
  - [1、APK资源分析](#1apk资源分析)
  - [2、APK编译(打包)、安装、反编译、热修复](#2apk编译打包安装反编译热修复)
    - [编译(打包)过程](#编译打包过程)
    - [gradle](#gradle)
    - [混淆](#混淆)
    - [数字签名](#数字签名)
    - [安装](#安装)
    - [反编译](#反编译)
    - [hook](#hook)
    - [热修复](#热修复)
    - [常见问题](#常见问题)
  - [3、同一套代码生成不同的应用](#3同一套代码生成不同的应用)
- [六、Android新技术](#六android新技术)
  - [1、AR](#1ar)
- [七、Android主流第三方库源码解析](#七android主流第三方库源码解析)
  - [1、OKHttp](#1okhttp)
  - [2、Retrofit](#2retrofit)
  - [3、Glide](#3glide)
  - [4、Rxjava](#4rxjava)
  - [5、GReenDao and LitePal](#5greendao-and-litepal)
  - [6、EventBus](#6eventbus)
  - [7、ARouter](#7arouter)
  - [支付](#支付)
  - [地图](#地图)
  - [推送](#推送)
  - [二维码](#二维码)
  - [PermissionX](#permissionx)
  - [XXPermission](#xxpermission)
- [八、开发中常用的库或其他快捷开发工具](#八开发中常用的库或其他快捷开发工具)
  - [xpopup](#xpopup)
- [九、Android日积月累](#九android日积月累)

---

# 一、Android系统原理

## Android的CPU架构

* [Android的CPU架构](https://github.com/geekist/developer_guide/blob/main/android/system/cpu.md)

## Android系统框架和启动过程

* [Android系统框架](https://developer.android.com/guide/platform?hl=zh-cn)
 
* [Android系统启动流程](https://github.com/geekist/developer_guide/blob/main/android/system/android系统启动流程.md)

* [Android Application启动流程](https://www.jianshu.com/p/a5532ecc8377)

* 结合优化部分的[AndroidApp启动分析与优化](https://github.com/geekist/developer_guide/blob/main/android/system/Android_App启动分析与优化.md)来了解App启动流程

## Android 进程间通信机制

* [Android系统进程间通讯机制---Binder](https://github.com/geekist/developer_guide/blob/main/android/system/binder.md)

* [Binder简介](https://blog.yorek.xyz/android/paid/zsxq/week11-binder/)

* [binder机制详细介绍](https://zhuanlan.zhihu.com/p/35519585)

* [ServiceManager介绍](https://blog.yorek.xyz/android/framework/binder2/)

* [AIDL实现跨进程通信](https://www.jianshu.com/p/29999c1a93cd)

## JNI与NDK

* [JNI编程](https://github.com/geekist/developer_guide/blob/main/android/system/jni.md)

* [JNI与NDK编程](https://blog.yorek.xyz/android/framework/JNI%E4%B8%8ENDK/)

## Android版本迭代

**Android版本兼容性**
* [Android平台版本兼容性问题总结](https://github.com/geekist/developer_guide/blob/main/android/compat/Android版本应用兼容性适配问题.md)

* [android 13的分区存储整理](https://github.com/geekist/developer_guide/blob/main/android/compat/android_13_storage.md) 


## Android厂商兼容性问题

**Android不同厂商系统兼容性**

* [android不同厂商问题合集](https://www.zhihu.com/question/65594088/answer/232791937)

# 二、Android Framework 基础知识

## 1、四大组件

### 1.1 四大组件的简介和启动过程

* [Activity][activity] （[Fragment][fragment]）---  [Activity启动过程--三个步骤，分两个层次描述](https://bbs.huaweicloud.com/blogs/168409)

* [Service][service] --- [Service启动流程分析](https://www.jiangkang.tech/2020/08/23/android/service-qi-dong-liu-cheng-fen-xi/)

* [ContentProvider][contentprovider] --- [ContentProvider启动过程](https://juejin.cn/post/6844903992514838541)

* [BroadcastReceiver][broadcastReceiver] ---[broadcastreceiver启动过程--简单描述，和activity类似](https://juejin.cn/post/6844903990329606158#heading-14)

* [Intent和Seriable以及Parcable](https://github.com/geekist/developer_guide/blob/main/android/4-components/Intent.md)

* [Context](https://www.jianshu.com/p/94e0f9ab3f1d)

* [四大组件启动过程](https://blog.yorek.xyz/android/framework/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E5%90%AF%E5%8A%A8%E8%BF%87%E7%A8%8B/)

[activity]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Activity.md
[fragment]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Fragment.md
[broadcastReceiver]:https://github.com/geekist/developer_guide/blob/main/android/4-components/BroadcastReceiver.md
[contentprovider]:https://github.com/geekist/developer_guide/blob/main/android/4-components/ContentProvider.md
[service]:https://github.com/geekist/developer_guide/blob/main/android/4-components/Service.md

## 2、View

### 2.1 android UI 控件的介绍基本用法

* [Android 控件介绍](https://github.com/geekist/developer_guide/blob/main/android/ui/ui控件介绍.md)


### 2.2 window和view

* [window和window manager](https://blog.yorek.xyz/android/framework/Window%E4%B8%8EWindowManager/)

* [Android View体系的简单介绍](https://juejin.cn/post/6844903970255667208)

* [View事件分发体系](https://blog.yorek.xyz/android/framework/View%E7%9A%84%E4%BA%8B%E4%BB%B6%E4%BD%93%E7%B3%BB/)

* [View的绘制原理](https://blog.yorek.xyz/android/framework/View%E7%9A%84%E7%BB%98%E5%88%B6%E5%8E%9F%E7%90%86/)[RemoteViews](https://blog.yorek.xyz/android/framework/RemoteViews/)

* [自定义View](https://github.com/geekist/developer_guide/blob/main/android/ui/自定义控件.md)


[self]:https://github.com/geekist/developer_guide/blob/main/android/ui/自定义控件.md

### 2.3 android对话框

* [android原生对话框开发](https://developer.android.com/guide/topics/ui/dialogs#java)

* [android原生8种对话框（Dialog）使用方法汇总](https://www.cnblogs.com/gzdaijie/p/5222191.html)

* [android底部对话框](https://www.jianshu.com/p/e1d2cc82e756)



### 3、动画

* [Animation](https://github.com/geekist/developer_guide/blob/main/android/ui/Animation.md)

## 4、通知
* [Notification][notification]


[notification]:https://github.com/geekist/developer_guide/blob/main/android/notification/Notification.md

## 5、多媒体

* [图片][photo]

* [Android图片相关知识总结](https://www.jianshu.com/p/8c1d3283ea1c)

* [音频][audio]

* [视频][video]

[photo]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Photo.md
[audio]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Audio.md
[video]:https://github.com/geekist/developer_guide/blob/main/android/multimedia/Video.md

## 6、数据存储

* [SharedPreference][sharedpreference] ---  [sharePreference存储优化](https://blog.yorek.xyz/android/paid/master/storage_1/)---腾讯[MMKV](https://github.com/Tencent/MMKV/blob/master/README_CN.md)


* [FileIO][fileio] ---  [file存储优化](https://blog.yorek.xyz/android/paid/master/storage_2/)


* [SQLite][sqlite] ---  [数据库sqlite存储优化](https://blog.yorek.xyz/android/paid/master/storage_3/)

[sharedpreference]:https://github.com/geekist/developer_guide/blob/main/android/database/SharedPreference.md
[fileio]:https://github.com/geekist/developer_guide/blob/main/android/database/FileIO.md
[sqlite]:https://github.com/geekist/developer_guide/blob/main/android/database/SQLite.md


## 7、网络编程（WebView）

* [webview](https://github.com/geekist/developer_guide/blob/main/android/ui/webview.md)

* [webview和Javascript交互](https://github.com/geekist/developer_guide/blob/main/android/ui/webview_h5.md)

* [第三方开源库agentweb封装webview实现带进度条的webview](https://github.com/Justson/AgentWeb)

## 8、Android消息机制与Android线程和线程池

### 8.1 线程和线程池

* [Java线程][javathread]

* [Android线程和线程池](https://blog.yorek.xyz/android/framework/Android%E7%BA%BF%E7%A8%8B%E4%B8%8E%E7%BA%BF%E7%A8%8B%E6%B1%A0/)

### 8.2消息机制--Handler、Looper，Message、MessageQueue

* [Android消息机制](https://blog.yorek.xyz/android/framework/Android%E6%B6%88%E6%81%AF%E6%9C%BA%E5%88%B6/) Activity.runOnUIThread(runable)以及View.post(runable)

* [Handler][handler]

* [HandlerThread][handlerthread]

### 8.3 Android的异步调用方法

* [AsyncTask][asynctask]

* [IntentService][intentservice]

[javathread]:https://github.com/geekist/developer_guide/blob/main/android/thread/JavaThread.md
[handler]:https://github.com/geekist/developer_guide/blob/main/android/thread/Handler.md
[handlerthread]:https://github.com/geekist/developer_guide/blob/main/android/thread/HandlerThread.md
[asynctask]:https://github.com/geekist/developer_guide/blob/main/android/thread/AsyncTask.md
[intentservice]:https://github.com/geekist/developer_guide/blob/main/android/thread/IntentService.md

## 9、Jetpack
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


# 三、Android性能调优


* [Android性能优化](https://blog.yorek.xyz/android/framework/%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96/)

## 1、启动优化

* [Activity启动过程-类分析](https://github.com/geekist/developer_guide/blob/main/android/system/android_app启动流程分析.md) ---  [Activity启动过程代码分析--三个步骤，分两个层次描述](https://bbs.huaweicloud.com/blogs/168409)

* [Android启动分析与优化](https://github.com/geekist/developer_guide/blob/main/android/system/Android_App启动分析与优化.md)

* [启动优化分析-1](https://blog.yorek.xyz/android/paid/master/start_1/) 

* [启动优化分析-2](https://blog.yorek.xyz/android/paid/master/start_2/)

* [启动优化的点](https://github.com/geekist/developer_guide/blob/main/android/system/启动优化的点.md)
* [四大组件启动过程代码分析](https://blog.yorek.xyz/android/framework/%E5%9B%9B%E5%A4%A7%E7%BB%84%E4%BB%B6%E5%90%AF%E5%8A%A8%E8%BF%87%E7%A8%8B/)

## 2、UI优化

**Android不同屏幕兼容性**

* [Android屏幕适配解决方案](https://www.jianshu.com/p/ec5a1a30694b)

* [屏幕适配方案总结](https://juejin.cn/post/6844903599495970830)

* [AndroidAutoSize代码分析](https://blog.csdn.net/u012588160/article/details/105876735) && [Autosize使用](https://www.xiaoheidiannao.com/220402.html)

## 3、ANR与崩溃定位与分析

* [崩溃优化分析](https://github.com/geekist/developer_guide/blob/main/android/system/android崩溃优化.md)

* [ANR问题的定位与分析](https://github.com/geekist/developer_guide/blob/main/android/system/android_ANR.md)

## 4、内存优化

* [Android内存管理与内存回收机制](https://www.jianshu.com/p/258229426da4)

* [Android内存优化技巧](https://www.jianshu.com/p/51e28a2c609c)

* [内存泄漏分析与查找](https://juejin.cn/post/6844904067534159880#heading-24)

* [LeakCanary使用详细教程](https://www.jianshu.com/p/c32f9aa85f18)

## 5、卡顿优化

* [深入探索Android卡顿优化-1](https://juejin.cn/post/6844904062610046990)

* [深入探索Android卡顿优化-2](https://juejin.cn/post/6844904066259091469)

## 6、耗电优化

* [androird耗电分析与优化-1](https://blog.yorek.xyz/android/paid/master/battery_1/)

* [androird耗电分析与优化-2](https://blog.yorek.xyz/android/paid/master/battery_2/)

* [Android性能优化系列之电量优化](https://blog.csdn.net/u012124438/article/details/74617649)

* [深入探索 Android 电量优化](https://juejin.cn/post/6844904195523346439)

## 7、存储优化

* [sharePreference存储优化](https://blog.yorek.xyz/android/paid/master/storage_1/)---腾讯[MMKV](https://github.com/Tencent/MMKV/blob/master/README_CN.md)

* [contentproviver存储优化](https://blog.yorek.xyz/android/paid/master/storage_1/)

* [file存储优化](https://blog.yorek.xyz/android/paid/master/storage_2/)

* [数据库sqlite存储优化](https://blog.yorek.xyz/android/paid/master/storage_3/)

## 8、进程保活
* [关于消息推送](https://www.mdeditor.tw/pl/pN5J)

* [Android进程保活](http://www.52im.net/thread-3033-1-1.html)

# 四、Android测试框架

* [Android自动化测试框架介绍](https://github.com/geekist/developer_guide/blob/main/android/test/autotest.md)

* [Andoid单元测试介绍](https://github.com/geekist/developer_guide/blob/main/android/test/unittest.md)

# 五、Android应用编译和发布

## 1、APK资源分析

* [Android.manifest文件介绍]()

## 2、APK编译(打包)、安装、反编译、热修复

### 编译(打包)过程

* [Java程序编译和运行原理](https://github.com/geekist/developer_guide/blob/main/android/studio/java编译运行原理.md)

* [Android打包apk的流程](https://github.com/geekist/developer_guide/blob/main/android/studio/android打包apk流程.md) **[命令行记忆](https://www.cnblogs.com/sjm19910902/p/6416022.html)**

* [Android打包Jar和aar流程](https://github.com/geekist/developer_guide/blob/main/android/studio/jar-and-aar.md)


* [Android MultiDex原理及实现记录](https://www.jianshu.com/p/1c5e8f281d0d)

* [启用MultiDex](https://github.com/geekist/developer_guide/blob/main/android/studio/MultiDex.md)

### gradle

* [gradle官网](https://docs.gradle.org/current/userguide/userguide.html)

* [build.gradle文件介绍](https://github.com/geekist/developer_guide/blob/main/android/studio/Gradle.md)

* [深入理解gradle编译-Android基础篇](https://blog.csdn.net/chouhuan1877/article/details/100808667)

* [gradle实现Androidapp的编译过程](http://www.voidcn.com/article/p-meuukmnh-bch.html)

### 混淆

* [Android 代码混淆配置总结](https://www.cnblogs.com/renhui/p/9299786.html)

* [Android App代码混淆终极解决方案](http://www.androidchina.net/8367.html)

* [android 混淆的项目实例](https://github.com/geekist/developer_guide/blob/main/android/studio/混淆实例.md)

### 数字签名

* [android 数字签名](./android/studio/android_signature.md)

### 安装

* [Android安装apk的流程](https://github.com/geekist/developer_guide/blob/main/android/studio/android打包apk流程.md)

* [Android版本检测和安装](https://github.com/geekist/developer_guide/blob/main/android/studio/Update.md)

* [Android Apk安装过程分析](https://www.jianshu.com/p/953475cea991)


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

* [切面编程](https://mp.weixin.qq.com/s?__biz=MzI0MjE3OTYwMg==&mid=2649550494&idx=1&sn=6982c5aee321ed731970c763708ae7ec&chksm=f11805e3c66f8cf57a02bde8bea85adf41b3aed795aab4feb08e599e71c0ee991b73a557c52c&mpshare=1&scene=1&srcid=0820H3HbpXCZMCMcYnhhZ8pL&sharer_sharetime=1598321087796&sharer_shareid=11fdebba4d506eab0c1cc6d88064dd1e&exportkey=AYQW7eWxwPr2ZnJQntTlbQ4=&pass_ticket=Zm5QkYXc5qNbpMJX3wz0O3EsJVGcfP55Vvi%2bLlz58prd1vsPjYwJ4xSQUL5Uds46&wx_header=0#rd)

* [如何生成签名文件---从jks转keystore文件](https://github.com/geekist/developer_guide/blob/main/android/studio/keystore.md)

## 3、同一套代码生成不同的应用

* [android开发build应用---官网介绍](https://developer.android.com/studio/build?hl=zh-cn)

* [debug和release的版本区别](./android/studio/debug_release.md)

* [gradle build中变量设置和读取的几种方法](./android/studio/gradle_variant.md)

* [实战一：自动生成连接测试服务器和正式服务器的应用](./android/studio/gradle_1.md)

* [实战二：同一套代码生成不同的应用程序_UI](./android/studio/gradle_2.md)

* [实战三：同一套代码生成不同的应用程序_分离第三方服务](./android/studio/gradle_3.md)

# 六、Android新技术 

## 1、AR

* [关于AR的基本概念和现状](https://blog.csdn.net/qq_32138419/article/details/106850796)

* [ARCore学习之旅：基础概念](https://juejin.cn/post/6844903493900173319)

* [ARCore总结整理](https://blog.csdn.net/yangwu007?t=1)


# 七、Android主流第三方库源码解析

## 1、OKHttp

* [okhttp框架设计](https://juejin.cn/post/6844903743951994887)---[okhttp拦截器分析](https://juejin.cn/post/6844904008646295566)

## 2、Retrofit

* [retrofit使用方法](https://www.jianshu.com/p/a3e162261ab6)---[retrofit源码解析](https://www.jianshu.com/p/0c055ad46b6c)

## 3、Glide
* [glide源码解析](https://juejin.cn/post/6844903605728706573) ---[主流图片加载库的比较](https://blog.csdn.net/carson_ho/article/details/51939774)---[guolin-blog](https://guolin.blog.csdn.net/article/details/53759439)

* [Glide常问的点](https://juejin.cn/post/6844903986412126216)
## 4、Rxjava
**Rxjava是一个薄弱环节，需要多加学习**

* [Rxjava][rxjava]

* [Rxjava使用方法](https://blog.csdn.net/carson_ho/article/details/78179340)---[Rxjava源码解析](https://blog.csdn.net/carson_ho/article/details/79168523)

* [Rxjava2系列](https://blog.csdn.net/carson_ho/category_7227390.html)

## 5、GReenDao and LitePal
* [GreenDao源码分析](https://jsonchao.github.io/2018/12/22/Android%E4%B8%BB%E6%B5%81%E4%B8%89%E6%96%B9%E5%BA%93%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90%EF%BC%88%E5%9B%9B%E3%80%81%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3GreenDao%E6%BA%90%E7%A0%81%EF%BC%89/)

* [Litepal][litepal]

## 6、EventBus
* [Eventbus](https://juejin.cn/post/6844904007199113229)


## 7、ARouter
* [ARouter使用介绍][arouter]--[ARouter源码分析](https://juejin.cn/post/6844903505937842183)

* [ARouter官方文档](https://developer.aliyun.com/article/71687)

[arouter]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ARouter.md
[eventbus]:https://github.com/geekist/developer_guide/blob/main/android/libraries/EventBus.md
[autosize]:https://github.com/geekist/developer_guide/blob/main/android/libraries/AndroidAutoSize.md
[okgo]:https://github.com/geekist/developer_guide/blob/main/android/libraries/OkGo.md
[rxjava]:https://github.com/geekist/developer_guide/blob/main/android/libraries/RxJava.md

[glide]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Glide.md

[litepal]:https://github.com/geekist/developer_guide/blob/main/android/libraries/LitePal.md

* [MediaPicker](https://github.com/geekist/developer_guide/blob/main/android/project/mediapicker.md)

## 支付

* [微信和支付宝android客户端接入流程](https://github.com/geekist/developer_guide/blob/main/pay/alipay.md)

## 地图

## 推送
[个推android接入流程](https://github.com/geekist/developer_guide/blob/main/android/3rd/getui.md)


## 二维码


## PermissionX

* [郭霖permissionX专栏](https://blog.csdn.net/guolin_blog/category_10108528.html)

* [使用方法](./android/3rd/permissionx.md)

## XXPermission

因为permissionX在升级到1.7.1时被腾讯安全管家检测出有病毒，换用XXPermission

* [Github地址](https://github.com/getActivity/XXPermissions)

* [Permission权限的基本介绍](https://www.jianshu.com/p/6a4dff744031)

* [XXPermission使用方法](https://www.jianshu.com/p/c69ff8a445ed)

permissionX -----Tecent 检测有病毒 A.fraud.SCMgeneric

easypermissions --- 无法定制自定义对话框

andpermission --- 停滞维护

xxpermission -- 尝试使用

# 八、开发中常用的库或其他快捷开发工具

## xpopup

* [第三方库xpopup](https://github.com/li-xiaojun/XPopup)

* [使用第三方库xpopup自定义dialog](https://github.com/geekist/developer_guide/blob/main/android/ui/xpopup.md)

# 九、Android日积月累

* [Android开发过程中的疑难杂症](https://github.com/geekist/developer_guide/blob/main/android/others/question.md)

* [日积月累](https://github.com/geekist/developer_guide/blob/main/android/diary.md)
