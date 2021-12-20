## Android 连接阿里云MQTT


## 配置SDK

* 配置阿里云仓库地址


在Android工程根目录下的基础配置文件./build.gradle中，加入阿里云仓库地址，配置仓库。
```
allprojects { 
   repositories { 
     jcenter()
     google()
     // 阿里云仓库地址
     maven { 
            url "http://maven.aliyun.com/nexus/content/repositories/releases/" 
        }
    }
}
```

* 添加SDK依赖

在模块的./build.gradle中，添加SDK的依赖，引入SDK :iot-linkkit。
 //阿里云云物联网
    implementation ('com.aliyun.alink.linksdk:iot-linkkit:1.7.2')
    implementation 'com.aliyun.alink.linksdk:iot-device-manager:1.7.2.2'
    implementation('com.aliyun.alink.linksdk:public-channel-core:0.7.7.1')
```


* 混淆配置

```


# linkkit API
-keep class com.aliyun.alink.**{*;}
-keep class com.aliyun.linksdk.**{*;}
-dontwarn com.aliyun.**
-dontwarn com.alibaba.**
-dontwarn com.alipay.**
-dontwarn com.ut.**

# keep native method
-keepclasseswithmembernames class * {
    native <methods>;
}

# keep netty
-keepattributes Signature,InnerClasses
-keepclasseswithmembers class io.netty.** {
    *;
}
-keepnames class io.netty.** {
    *;
}
-dontwarn io.netty.**
-dontwarn sun.**

# keep mqtt
-keep public class org.eclipse.paho.**{*;}

# keep fastjson
-dontwarn com.alibaba.fastjson.**
-keep class com.alibaba.fastjson.**{*;}

# keep gson
-keep class com.google.gson.** { *;}

# keep network core
-keep class com.http.**{*;}
-keep class org.mozilla.**{*;}

# keep okhttp
-dontwarn okhttp3.**
-dontwarn okio.**
-dontwarn javax.annotation.**
-dontwarn org.mozilla.**

-keep class okio.**{*;}
-keep class okhttp3.**{*;}
-keep class org.apache.commons.codec.**{*;}

-keep class com.aliyun.alink.devicesdk.demo.FileProvider{*;}
-keep class android.support.**{*;}
-keep class android.os.**{*;}
```

* 配置AndroidManifest.xml文件



打开Android工程根目录下的./AndroidManifest.xml，在manifest一栏添加如下代码。

```
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    ....
在application一栏添加如下代码。
    <application
        tools:replace="android:allowBackup"

```