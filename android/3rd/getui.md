
# 个推接入流程

* [个推Android sdk 官方文档](https://docs.getui.com/getui/mobile/android/androidstudio/)

## 一、推送流程技术方案
如下图所示：
![](./assets/getui_1.png)

## 二、环境配置

* 1、运行环境

本SDK支持Android 4.0及以上版本的Android系统；

支持Phone、TV、机顶盒等其他基于Android系统的智能设备；

* 2、权限控制：

必选权限

（1）网络连接（必选）

    <uses-permission android:name="android.permission.INTERNET”/>

（2）获取手机状态参数，并作为生成个推唯一标识的必要参数（必选）

   <uses-permission android:name="android.permission.READ_PHONE_STATE”/>

（3）查看网络状态，sdk重连机制等需要使用（必选）

    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE”/>

（4）查看wifi连接状态（必选）

    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE”/>

（5）开机自启动权限，提升sdk活跃，保障触达（必选）

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED”/>

（6）写sd卡权限，做数据备份（必选）

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE”/>

（7）震动权限（使用通知功能必选）

    <uses-permission android:name="android.permission.VIBRATE”/>

（8）获取任务信息，目的是防止sdk被频繁唤醒（必选）

    <uses-permission android:name="android.permission.GET_TASKS”/>

可选权限

（1）支持电子围栏功能（可选）

    <uses-permission android:name="android.permission.BLUETOOTH"/>
    <uses-permission android:name="android.permission.BLUETOOTH_ADMIN"/>
    <uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION”/>

（2）自定义权限，为了防止小部分手机服务没法正常工作（可选）

    <uses-permission android:name="getui.permission.GetuiService.${applicationId}"/>
    <permission
        android:name="getui.permission.GetuiService.${applicationId}"
        android:protectionLevel="normal"/>

* 3、配置maven库地址：

```xml
buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        ......
    }
}

allprojects {
    repositories {
        jcenter()
        google()
        maven {
            url "http://mvn.gt.getui.com/nexus/content/repositories/releases/"
        }
    }
}

```
* 4、配置依赖项：

注意：如果项目中其他 so 库只支持其中某几种 CPU 架构，那么应该根据其他 so 库所支持的 CPU 架构的最小集来配置。否则如果在特定架构上未能支持所有 so 库，则很可能导致程序运行异常。切记！

在 app/build.gradle 文件中的 android.defaultConfig 下指定所需的 CPU 架构，如下所示：



```xml
defaultConfig {
    ndk {
        // 注意：这里需要添加项目所需 CPU 类型的最小集
        abiFilters "armeabi", "armeabi-v7a", "x86_64", "x86"
    }
}
......
```

配置 SDK 依赖及应用参数：在 app/build.gradle 文件的 dependencies 块中引用个推 SDK 依赖 implementation 'com.getui:gtsdk:${version}'，此处的 ${version} 为对应的 SDK 版本号，并在android.defaultConfig 下添加 manifestPlaceholders，配置个推相关的应用参数， 如下所示：

```xml
    ......

    android {
        defaultConfig {
            manifestPlaceholders = [
                  //从 3.1.2.0 版本开始，APPID 占位符从 GETUI_APP_ID 切换为 GETUI_APPID 
                  //后续所有产品的 APPID 均统一配置为 GETUI_APPID 占位符
                GETUI_APPID       : "your appid",

                  //渠道若为纯数字则不能超过 int 表示的范围。 
                GT_INSTALL_CHANNEL      : "your channel"
            ]        

            ndk {
                // 添加项目所需 CPU 类型的最小集
                abiFilters "armeabi", "armeabi-v7a", "x86_64", "x86"
            }
        }
      ......
    }

    dependencies {
        implementation 'com.getui:gtsdk:3.1.4.0'  //个推SDK
        implementation 'com.getui:gtc:3.1.0.0'  //个推核心组件
    }  


```

若 APPID 值一致且只使用了 GETUI_APPID 占位符, 如果集成了推送, 则务必要在manifest中补充如下占位符：

```xml
<meta-data
    android:name="PUSH_APPID"
    android:value="个推SDK的appid" />
```

* 5、配置推送服务
为了让推送服务在部分主流机型上更稳定运行，从 2.9.5.0 版本开始，个推支持第三方应用配置使用自定义 Service 来作为推送服务运行的载体。

在项目源码中添加一个继承自 com.igexin.sdk.PushService 的自定义 Service：

```java
package com.getui.demo.service;
public class DemoPushService extends com.igexin.sdk.PushService {
}
```

在 AndroidManifest.xml 中添加上述自定义 Service，（使用 maven 集成，android:process 属性必须为 pushservice。手动集成方式也请保证与其他组件进程名一致，建议复制本文档的默认配置即可），如下：

```xml
 <!-- 请根据您当前自定义的 PushService 名称路径进行配置-->
  <service
      android:name="com.getui.demo.service.DemoPushService"
      android:exported="true"
      android:label="PushService"
      android:process=":pushservice"/>
```

## 三、使用个推服务

* 1、初始化 SDK

调用个推初始化代码：

```java
com.igexin.sdk.PushManager.getInstance().initialize(Context context) 
```

进行 SDK 的初始化。我们建议开发者在 Application.onCreate() 和主 Activity.onCreate() 方法中初始化个推 SDK。多次调用 SDK 初始化并无影响。为了保证 SDK 服务稳定，推荐引导用户授权相关的隐私权限。

另外，为了保证推送通知更好的触达用户，降低用户对于通知开关设置的难度，我们建议在应用代码中引导用户前往通知页面打开允许应用通知开关，具体实现代码可以参考 Demo 工程。

* 2、自定义接收推送服务事件

在项目源码中添加一个继承自 com.igexin.sdk.GTIntentService 的类，用于接收 CID、透传消息以及其他推送服务事件。请参考下列代码实现各个事件回调方法：

```java
package com.getui.demo;

import android.content.Context;
import android.util.Log;

import com.igexin.sdk.GTIntentService;
import com.igexin.sdk.message.GTCmdMessage;
import com.igexin.sdk.message.GTNotificationMessage;
import com.igexin.sdk.message.GTTransmitMessage;

/**
 * 继承 GTIntentService 接收来自个推的消息，所有消息在线程中回调，如果注册了该服务，则务必要在 AndroidManifest 中声明，否则无法接受消息
 */
public class DemoIntentService extends GTIntentService {

    @Override
    public void onReceiveServicePid(Context context, int pid) {
    }

    // 处理透传消息
    @Override
    public void onReceiveMessageData(Context context, GTTransmitMessage msg) {
        // 透传消息的处理，详看 SDK demo
    }

    // 接收 cid
    @Override
    public void onReceiveClientId(Context context, String clientid) {
        Log.e(TAG, "onReceiveClientId -> " + "clientid = " + clientid);
    }

    // cid 离线上线通知
    @Override
    public void onReceiveOnlineState(Context context, boolean online) {
    }

    // 各种事件处理回执
    @Override
    public void onReceiveCommandResult(Context context, GTCmdMessage cmdMessage) {
    }

    // 通知到达，只有个推通道下发的通知会回调此方法
    @Override
    public void onNotificationMessageArrived(Context context, GTNotificationMessage msg) {
    }

    // 通知点击，只有个推通道下发的通知会回调此方法
    @Override
    public void onNotificationMessageClicked(Context context, GTNotificationMessage msg) {   
    }
}
```


在 AndroidManifest.xml 中配置上述 IntentService 类，如下：

```xml
<service
    android:name="com.getui.demo.service.DemoIntentService"
    android:permission="android.permission.BIND_JOB_SERVICE"/>
```

* 3、 验证推送
 
查看调试日志信息

在 Application 的 onCreate 中添加以下代码：

```java
com.igexin.sdk.PushManager.getInstance().setDebugLogger(this, new IUserLoggerInterface() {
    @Override
    public void log(String s) {
        Log.i("PUSH_LOG",s);
    }
});
```

连接手机或启动 Android 模拟器，编译运行你的工程，查看 logcat 信息。过滤 logcat 中的 PUSH_LOG 信息，如果看到 Login successed with cid = xxx 日志输出，则说明 SDK 初始化成功。

```xml
    [GT-PUSH] [PushManager]Start initializing sdk
    [GT-PUSH] [PushManager]start pushService = com.getui.demo.DemoPushServiceNew
    [GT-PUSH] [LOG-LogController] Sdk version = 3.0.1.0
    [GT-PUSH] [PushManager]call registerPushIntentService
    [GT-PUSH] onHandleIntent() = get sdk service pid 
    [GT-PUSH] ServiceManager start from initialize...
    [GT-PUSH] load so = getuiext3 by system success
    [GT-PUSH] Start login appid = TI85ilD*******L89MFNV appkey = 1qNlp*******BP9COgYjA
    [GT-PUSH] Login successed with cid = e3f004e873f9a5d9e2e006c4a9ca2f5f
    [GT-PUSH] onHandleIntent() = received client id 
```

注意：com.igexin.sdk.PushManager.getInstance().setDebugLogger 接口仅限调试的时候使用，切勿发布到线上版本，重复调用仅以第一次为准。 可以任意顺序调用该接口与个推初始化接口，但建议紧邻着个推初始化接口调用该接口。 如果看到 Warning! the log cache is too long to show the full content,we suggest you call initialize and setDebugLogger in a short time interval. 这样的提示请调整调用时间间隔。








