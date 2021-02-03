

# MultiDex

*[AndroidDex分包方案](https://www.jianshu.com/p/a67a560903fa)

## 关于 64K 引用限制
Android 应用 (APK) 文件包含 Dalvik Executable (DEX) 文件形式的可执行字节码文件，这些文件包含用来运行应用的已编译代码。Dalvik Executable 规范将可在单个 DEX 文件内引用的方法总数限制为 65536，其中包括 Android 框架方法、库方法以及您自己的代码中的方法。在计算机科学领域内，术语千（简称 K）表示 1024（即 2^10）。由于 65536 等于 64 X 1024，因此这一限制称为“64K 引用限制”。

## Android 5.0 之前版本的 MultiDex 支持
Android 5.0（API 级别 21）之前的平台版本使用 Dalvik 运行时执行应用代码。默认情况下，Dalvik 将应用限制为每个 APK 只能使用一个 classes.dex 字节码文件。为了绕过这一限制，您可以在项目中添加 MultiDex 支持库：

* 1、添加Androidx的依赖项：

```java
dependencies {
    def multidex_version = "2.0.1"
    implementation 'androidx.multidex:multidex:$multidex_version'
}

android {
    defaultConfig {
        ...
        minSdkVersion 15
        targetSdkVersion 28
        `multiDexEnabled true`
    }
    ...
}
   
```

* 2、配置Application

```java
有三种方法可以配置application
如果您不替换 Application 类，请修改清单文件以设置 <application> 标记中的 android:name，如下所示：

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
    <application
            android:name="android.support.multidex.MultiDexApplication" >
        ...
    </application>
</manifest>


如果您替换 Application 类，请对其进行更改以扩展 MultiDexApplication（如果可能），如下所示：
class MyApplication : MultiDexApplication() {...}

或者，如果您替换 Application 类，但无法更改基类，则可以改为替换 attachBaseContext() 方法并调用 MultiDex.install(this) 以启用 MultiDex：

class MyApplication : SomeOtherApplication() {

    override fun attachBaseContext(base: Context) {
        super.attachBaseContext(base)
        MultiDex.install(this)
    }
}





```






## Android 5.0 及更高版本的 MultiDex 支持
Android 5.0（API 级别 21）及更高版本使用名为 ART 的运行时，它本身支持从 APK 文件加载多个 DEX 文件。ART 在应用安装时执行预编译，扫描 classesN.dex 文件，并将它们编译成单个 .oat 文件，以供 Android 设备执行。因此，如果您的 minSdkVersion 为 21 或更高的值，则默认情况下会启用 MultiDex，并且您不需要 MultiDex 支持库。