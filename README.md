
# developer_guide

本文档包含以下几个部分：

## Android编程规范

为使代码规范、可读性强，提高开发效率和方便沟通，参照官方的文档，并结合自身开发的经验，总结出的适合个人和团队使用的编程规范。

* [Android-Java编程规范][java-rule]

* [Android-Kotlin编程规范][kotlin-rule]

[java-rule]: https://github.com/geekist/developer_guide/blob/main/rules/Android-Java编程规范.md

[kotlin-rule]: https://github.com/geekist/developer_guide/blob/main/rules/Android-Kotlin编程规范.md

## Android项目基本框架
关于组件化开发、程序架构设计、项目配置、编译和发布流程的一个基本框架。

[Android项目基本框架][architecture]

[architecture]: https://github.com/geekist/developer_guide/blob/main/architecture/Android项目基本框架.md

## Android开发中流行的框架库

[安卓常用框架汇总](http://www.androidchina.net/9941.html)


开发过程中默认使用的第三方流行框架。

* [ARouter][arouter]

* [EventBus][eventbus]

* [RxJava][rxjava]

* [Fragmentation](https://github.com/YoKeyword/Fragmentation/wiki)

依赖注入的框架

* [Dagger2](https://github.com/geekist/developer_guide/blob/main/android/libraries/dagger2.md)

* [Koin](https://start.insert-koin.io/#/)


[arouter]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ARouter.md
[eventbus]:https://github.com/geekist/developer_guide/blob/main/android/libraries/EventBus.md


[rxjava]:https://github.com/geekist/developer_guide/blob/main/android/libraries/RxJava.md


[fragmentation]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Fragmentation.md

## Android开发中优秀的第三方控件或工具
项目中经常会使用到的优秀的第三方控件或开发工具

网络传输相关

- retrofit

- [OkGo][okgo]

- gson

权限相关

- [Android危险权限列表](https://blog.csdn.net/xxdw1992/article/details/89811370)

- 运行时动态申请权限的方法以及一个开源库[AndPermission][andpermission]

[andpermission]:https://github.com/geekist/developer_guide/blob/main/android/libraries/AndPermission.md

- [PermissionX][permissionx]-- [github地址](https://github.com/guolindev/PermissionX)

[permissionx]:https://guolin.blog.csdn.net/article/details/106181780

UI控件相关


通过标签直接生成shape，无需再写shape.xml

* [backgroundLibrary](https://github.com/JavaNoober/BackgroundLibrary)

对话框


- [XPopup](https://github.com/geekist/developer_guide/blob/main/android/libraries/XPopup.md)

沉浸式状态栏

- [ImmersionBar][immersionbar]
[immersionbar]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ImmersionBar.md

搜索框

- [searchLayout](https://github.com/Carson-Ho/Search_Layout)

TableLayout

-[FlycoTabLayout](https://github.com/H07000223/FlycoTabLayout)

下拉刷新

- [SmartRefreshLayout][smartrefresh]

图片相关

- Sketch

- [Glide](https://guolin.blog.csdn.net/article/details/53759439)

数据库相关

- [LitePal][litepal]

浏览器相关

- [WebView](https://blog.csdn.net/carson_ho/article/details/52693322)

- [AgentWeb](https://github.com/Justson/AgentWeb)

- pgyersdk

- pgyersdk

- dns

[okgo]:https://github.com/geekist/developer_guide/blob/main/android/libraries/OkGo.md
[smartrefresh]:https://github.com/geekist/developer_guide/blob/main/android/libraries/SmartRefreshLayout.md

[glide]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Glide.md

[litepal]:https://github.com/geekist/developer_guide/blob/main/android/libraries/LitePal.md

- sweet alert dialog

## Android扩展功能导入
地图、支付、消息推送、二维码扫描等外部功能接入

* [地图功能接入][map]

* [支付功能接入][pay]


* [消息推送功能接入][push]


* [二维码扫描功能接入][scan-code]

[map]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Map.md

[pay]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Pap.md

[push]:https://github.com/geekist/developer_guide/blob/main/android/libraries/Push.md

[scan-code]:https://github.com/geekist/developer_guide/blob/main/android/libraries/ScanCode.md


## Android基础类库
开发过程中长期整理的工具代码


[带有倒计时功能的闪屏页面Splash Activity](https://www.jianshu.com/p/b38ec0bfee7d)      ---[github地址](https://github.com/ImportEffort/SplashActivityDemo)

[带有广告的闪屏页面Splash Activity](https://developers.adnet.qq.com/doc/android/union/union_splash)


[Utils Code][utils]

[utils]:https://github.com/geekist/developer_guide/blob/main/utils/Utils.md

## Android TCP/IP 开发
* [Http概述](https://github.com/geekist/developer_guide/blob/main/network/Http.md)
 
* [Tcp概述](https://github.com/geekist/developer_guide/blob/main/network/tcp.md)

* [Android开发TCP](https://github.com/geekist/developer_guide/blob/main/network/tcp_android.md)

* [tcpclient.java](https://github.com/geekist/developer_guide/blob/main/network/TCPClient.java)

* [tcpserver.java](https://github.com/geekist/developer_guide/blob/main/network/TCPServer.java)

## Android UDP 开发

* [UDP概述](https://github.com/geekist/developer_guide/blob/main/network/udp.md)

* [Android开发UDP](https://github.com/geekist/developer_guide/blob/main/network/udp_android.md)

* [UDPDemo](https://github.com/geekist/developer_guide/blob/main/network/UdpHub.java)

## [Android Wifi开发总结](https://blog.csdn.net/qq_34773981/article/details/79163579)

## Android Ble 开发
* [Android BLE 蓝牙开发入门](https://www.jianshu.com/p/3a372af38103)

* [fastble](https://github.com/Jasonchenlijian/FastBle/wiki)

* [rxble](https://github.com/Belolme/RxBLE)

* [Android 蓝牙开发技术分析](https://github.com/geekist/developer_guide/blob/main/bluetooth/bluetooth.md)


## Android IoT开发
* [MQTT基本介绍](https://github.com/geekist/developer_guide/blob/main/IoT/MQTT.md)

* [阿里云IOT开发](https://github.com/geekist/developer_guide/blob/main/IoT/Android连接阿里云MQTT.md)

## 硬件基础

* [硬件基础知识](https://github.com/geekist/developer_guide/blob/main/IoT/硬件基础知识.md)

## 基础知识学习
开发过程中的笔记整理，目的是为了唤醒记忆，达到以最快的速度保持知识栈的目的。

[Android][android]

[Kotlin][kotlin]

[android]:https://github.com/geekist/developer_guide/blob/main/android/android.md
[kotlin]:https://github.com/geekist/developer_guide/blob/main/kotlin/kotlin.md

## 作品开发

[Mercury]([kotlin]:https://github.com/geekist/developer_guide/blob/main/mercury/mercury.md)