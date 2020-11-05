## Android 连接阿里云MQTT


## 1、在App的gradle文件中，添加Paho Android Client依赖？
```java
dependencies {
    implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.1.0'
    implementation 'org.eclipse.paho:org.eclipse.paho.android.service:1.1.1'
    implementation 'androidx.legacy:legacy-support-v4:1.0.0'
}
```
这里注意最后一个依赖项，如果不加则崩溃！！！
## 2、在Android.manifest文件中，添加Paho Android Service信息：
```java
<!-- Mqtt Service -->
<service android:name="org.eclipse.paho.android.service.MqttService">
</service>
```

并且添加Paho MQTT Service所需的权限。
```java
<uses-permission android:name="android.permission.WAKE_LOCK" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```

## 3、下载AiotMqttOption.java到本地，用来计算username、password和clientId。

