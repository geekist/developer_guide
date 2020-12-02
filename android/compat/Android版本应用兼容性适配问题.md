# Android版本兼容性问题

## Android6.0以后，android对系统权限申请进行了分类：普通权限和危险权限
一类是普通权限，一类是危险权限。
普通权限只需要在manifest文件中申请即可，危险权限需要在运行时申请。
因此，开发者需要进行一些额外的工作：
1、在适当的时候申请权限
2、调用该功能之前判断权限
3、如果用户一直拒绝，则应该友好地屏蔽该功能。


## Android 6.0 中，谷歌取消了对 Apache HTTP 客户端的支持
```xml
 <uses-library
            android:name="org.apache.http.legacy"
            android:required="false" />
```
 
兼容性影响
所有的targetSdkVersion>=P的应用不适配的话，继续按照之前的方式使用apache http客户端会导致应用因为找不到apache http类抛异常崩溃；
小部分targetSdkVersion<P的应用，如果应用使用了非标准的classloader，不适配的话也是会导致闪退的问题。
异常日志：

08-24 10:47:10.455  4364  4364 E AndroidRuntime: java.lang.NoClassDefFoundError: Failed resolution of: Lorg/apache/http/client/methods/HttpGet;

适配指导
继续使用apache http客户端
targetSdkVersion<p的应用，如果测试发现有该问题，可能是显示指定了系统的ClassLoader去查找apache-http类，但是系统的ClassLoader已经找不到apache的类了，所以报错。所以建议应用如果还需要继续使用apache-http的类，不要显示指定系统的ClassLoader去加载apache-http的类，通过应用的ClassLoader去加载是没有问题的。
targetSdkVersion>=P的应用：
对于targetSdkVersion>=P的应用如果想继续使用apache-http客户端：

为了能编译通过需要在 build.gradle 文件中声明以下编译时依赖项：
android {
    useLibrary 'org.apache.http.legacy'
}

需要在应用的AndroidManifest.xml文件中添加：
<uses-library android:name="org.apache.http.legacy" android:required="false"/>

注意：对于最低SDK为23或更低的应用程序，android：required =“false”属性是必需的，因为在API等级低于24的设备上，org.apache.http.legacy库不可用。 （在这些设备上，Apache HTTP类在bootclasspath上可用。）

不再使用apache-http客户端
使用HttpURLConnection替代apache-http

## 从Android9.o开始，应用程序默认只能使用https的网络请求。
为了允许http的请求可以继续运行，需要创建一个xml文件，配置网络安全设置
new一个xml目录，创建一个network_config.xml文件
```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config xmlns:tools="http://schemas.android.com/tools">
    <base-config
        cleartextTrafficPermitted="true"
        tools:ignore="InsecureBaseConfiguration" />
</network-security-config>
```
然后在Android.manifest文件中添加配置：
```xml
<application
        android:name=".App"
        
        android:networkSecurityConfig="@xml/network_config" 
```


