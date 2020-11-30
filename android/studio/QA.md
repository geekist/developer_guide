
# Android Studio 常见问题

## Android Studio升级后如何导入旧版本工程？

 * 1、首先在gradle/wapper下面打开gradle-wrapper.properties文件，将下面一行替换成当前工程的版本：

```java
distributionUrl=https\://services.gradle.org/distributions/gradle-2.14.1-all.zip
distributionUrl=https\://services.gradle.org/distributions/gradle-6.5-bin.zip
```

* 2、修改library和app模块下面的build.gradle文件，将编译版本号修改为当前版本号，如果有compile关键字，改为implementation关键字。
```java
android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.yanbober.android_eventbus_demo"
        minSdkVersion 21
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'de.greenrobot:eventbus:2.4.0'

```
* 3、如果有旧版本的appcompat或者其他依赖，改为androidx的依赖

```java

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'de.greenrobot:eventbus:2.4.0'

```

* 4、在工程的build.gradle文件中，将classpath修改为当前的版本号,添加google仓库选择，

```java

 dependencies {
        classpath "com.android.tools.build:gradle:4.1.0"
```

```java

  repositories {
        google()
        jcenter()
    }

```

5、工程下的gradle.properties文件，如果没有添加“androidx”支持，改为支持
```java

org.gradle.jvmargs=-Xmx2048m

android.useAndroidX=true

android.enableJetifier=true

```

* 6、关闭工程重新打开即可。

## Android studio断点调试，没法断点进入切换线程代码的解决办法

1、先在子线程的代码打个断点，类似这样：
2、然后，在那个断点标识的小红点，右键，然后会弹出这个对话框：
3、默认一般是Thread，选择All，确认setDefault，然后重新编译一遍就OK了


