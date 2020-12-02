
# build.gradle介绍

## Gradle简介

Android Studio是采用gradle来构建项目的，Gradle 是用了一种基于 Groovy 的领域特定语言（DSL，Demain Specific Language）来声明项目设置，摒弃了 XML（如 ANT 和 Maven）的各种繁琐配置。

## 工程目录下的build.gradle文件

全局的build.grade主要设置的是声明仓库源，gradle的版本号说明等。
```java
// Top-level build file where you can add configuration options common to all sub-projects/modules.

apply from: "config.gradle"

buildscript {
    ext.kotlin_version = "1.4.10"
    repositories {
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/google' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/jcenter' }
        maven { url "https://jitpack.io" }
        maven { url 'https://dl.bintray.com/umsdk/release' }
        maven { url 'https://maven.google.com' }

        google()
        mavenCentral()
        maven { url 'https://maven.google.com' }
        jcenter()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:4.1.0"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/google' }
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }
        maven { url 'http://maven.aliyun.com/nexus/content/repositories/jcenter' }
        maven { url "https://jitpack.io" }
        maven { url 'https://dl.bintray.com/umsdk/release' }

        google()
        mavenCentral()
        maven { url 'https://maven.google.com' }
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}


```

* apply from

自定义的配置文件的引入。

* buildscript{}

闭包里是gradle脚本执行所需依赖，分别是对应的maven库和插件。


* allprojects{}

闭包里是项目本身需要的依赖，比如项目所需的maven库。


* task clean(type: Delete){}

是运行gradle clean时，执行此处定义的task任务，该任务继承自Delete，删除根目录中的build目录。


buildscript包含repositories闭包和dependencies闭包。

* repositories{}

配置远程仓库该闭包中声明了jcenter()和google()的配置，其中jcenter是一个代码托管仓库，上面托管了很多Android开源项目，在这里配置了jcenter后我们可以在项目中方便引用jcenter上的开源项目，从Android Studio3.0后新增了google()配置，可以引用google上的开源项目。

* dependencies{}

配置构建工具该闭包使用classpath声明了一个Gradle插件，由于Gradle并不只是用来构建Android项目，因此此处引入相关插件来构建Android项目，其中'3.0.0'为该插件的版本号，可以根据最新的版本号来调整。

**gradle本身的插件在工程目录下gradle/wrapper/gradle-wrapper.properities文件中**

```java
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.1.1-all.zip
```

## 模块(module)目录下的build.gradle文件
从文件内容可以看出，主要分为三大部分：

```java
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'

android {
    compileSdkVersion 30
    buildToolsVersion "30.0.2"

    defaultConfig {
        applicationId "com.jon.basic"
        minSdkVersion 16
        targetSdkVersion 30
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    implementation fileTree(dir: "libs", include: ["*.jar"])
    implementation 'androidx.appcompat:appcompat:1.2.0'
    implementation 'com.google.android.material:material:1.2.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.0.2'
    implementation 'androidx.navigation:navigation-fragment:2.3.0'
    implementation 'androidx.navigation:navigation-ui:2.3.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test.ext:junit:1.1.2'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.3.0'


    // Add Dagger dependencies
    api 'com.google.dagger:dagger:2.7'
        annotationProcessor 'com.google.dagger:dagger-compiler:2.7'
    implementation "androidx.core:core-ktx:+"
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
}

```
### apply plugin
Android Studio 4.1后改为了plugins
```java
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
}
```

* 'com.android.application'，表示该模块为应用程序模块，可以直接运行，打包得到的是.apk文件

* 'com.android.library'，表示该模块为库模块，只能作为代码库依附于别的应用程序模块来运行，打包得到的是.aar文件

* 'kotlin-android' kotlin的插件

* 'kotlin-android-extensions' kotlin的扩展插件，用来读取view中的配置

* 'kotlin-kapt' annotation注解插件

### android{}闭包

这个闭包主要为了配置项目构建的各种属性：


#### 编译时Android版本信息和构建工具版本信息

```java
compileSdkVersion rootProject.ext.android["compileSdkVersion"]
buildToolsVersion rootProject.ext.android["buildToolsVersion"]
```

compileSdkVersion：设置编译时用的Android版本
buildToolsVersion：设置编译时使用的构建工具的版本


#### 签名配置闭包

```java
    signingConfigs {// 自动化打包配置
        release {// 生产环境
            keyAlias 'test'
            keyPassword '123456'
            storeFile file('test.keystore')
            storePassword '123456'
        }
        debug {// 开发环境
            keyAlias 'test'
            keyPassword '123456'
            storeFile file('test.keystore')
            storePassword '123456'
        }
    }

```
签名配置完成后可以方便带签名打包，在module的Build Variants中有两个Type，分别是debug和release，可以选择任意一个类型进行打包，并且他们会利用各自配置的Key进行打包，执行 Run app或者Build->Build apk就会自动在module name/app/build/outputs/apk路径下生成Apk文件。

另一种打包方式是Build->Generate Signed APK填写签名信息生成Apk。

#### defaultConfig闭包
用来指定android apk的版本信息
```java
    compileSdkVersion 27//设置编译时用的Android版本
    defaultConfig {
        applicationId "com.billy.myapplication"//项目的包名
        minSdkVersion 16//项目最低兼容的版本
        targetSdkVersion 27//项目的目标版本
        versionCode 1//版本号
        versionName "1.0"//版本名称
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"//表明要使用AndroidJUnitRunner进行单元测试
    }

```

* applicationId：指定了项目的包名。


* minSdkVersion：指定项目最低兼容的版本，如果设备小于这个版本或者大于maxSdkVersion(一般不用)将无法安装这个应用，这里指定为16，表示最低兼容到Android 4.1系统。


* targetSdkVersion：指定项目的目标版本，表示在该目标版本上已经做过充分测试，系统会为该应用启动一些对应该目标系统的最新功能特性，Android系统平台的行为变更，只有targetSdkVersion的属性值被设置为大于或等于该系统平台的API版本时，才会生效。例如，若指定targetSdkVersion值为22，则表示该程序最高只在Android5.1版本上做过充分测试，在Android6.0系统上（对应targetSdkVersion为23）拥有的新特性如系统运行时权限等功能就不会被启用。

* versionCode：表示版本号，一般每次打包上线时该值只能增加，打包后看不见。

* versionName：表示版本名称，展示在应用市场上。

* testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"表明要使用AndroidJUnitRunner进行单元测试。

#### buildTypes{}闭包
buildTypes闭包用来指定生成安装文件的配置。一般分为debug和release两个闭包
```java

    buildTypes {// 生产/测试环境配置
        release {// 生产环境
            buildConfigField("boolean", "LOG_DEBUG", "false")//配置Log日志
            buildConfigField("String", "URL_PERFIX", "\"https://release.cn/\"")// 配置URL前缀
            minifyEnabled false//是否对代码进行混淆
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'//指定混淆的规则文件
            signingConfig signingConfigs.release//设置签名信息
            pseudoLocalesEnabled false//是否在APK中生成伪语言环境，帮助国际化的东西，一般使用的不多
            zipAlignEnabled true//是否对APK包执行ZIP对齐优化，减小zip体积，增加运行效率
            applicationIdSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
            versionNameSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
        }
        debug {// 测试环境
            buildConfigField("boolean", "LOG_DEBUG", "true")//配置Log日志
            buildConfigField("String", "URL_PERFIX", "\"https://test.com/\"")// 配置URL前缀
            minifyEnabled false//是否对代码进行混淆
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'//指定混淆的规则文件
            signingConfig signingConfigs.debug//设置签名信息
            debuggable false//是否支持断点调试
            jniDebuggable false//是否可以调试NDK代码
            renderscriptDebuggable false//是否开启渲染脚本就是一些c写的渲染方法
            zipAlignEnabled true//是否对APK包执行ZIP对齐优化，减小zip体积，增加运行效率
            pseudoLocalesEnabled false//是否在APK中生成伪语言环境，帮助国际化的东西，一般使用的不多
            applicationIdSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
            versionNameSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
        }
    }

```

* minifyEnabled：表明是否对代码进行混淆，true表示对代码进行混淆，false表示对代码不进行混淆，默认的是false。

* proguardFiles：指定混淆的规则文件，这里指定了proguard-android.txt文件和proguard-rules.pro文件两个文件，

* proguard-android.txt文件为默认的混淆文件，里面定义了一些通用的混淆规则。proguard-rules.pro文件位于当前项目的根目录下，可以在该文件中定义一些项目特有的混淆规则。


* buildConfigField：用于解决Beta版本服务和Release版本服务地址不同或者一些Log打印需求控制的。例如：配置buildConfigField("boolean", "LOG_DEBUG", "true")，这个方法接收三个非空的参数，第一个：确定值的类型，第二个：指定key的名字，第三个：传值，调用的时候BuildConfig.LOG_DEBUG即可调用。


* debuggable：表示是否支持断点调试，release默认为false，debug默认为true。


* jniDebuggable：表示是否可以调试NDK代码，使用lldb进行c和c++代码调试，release默认为false


* signingConfig：设置签名信息，通过signingConfigs.release或者signingConfigs.debug，配置相应的签名，但是添加此配置前必须先添加signingConfigs闭包，添加相应的签名信息。


* renderscriptDebuggable：表示是否开启渲染脚本就是一些c写的渲染方法，默认为false。


* renderscriptOptimLevel：表示渲染等级，默认是3。


* pseudoLocalesEnabled：是否在APK中生成伪语言环境，帮助国际化的东西，一般使用的不多。


* applicationIdSuffix：和defaultConfig中配置是一的，这里是在applicationId 中添加了一个后缀，一般使用的不多。


* versionNameSuffix：表示添加版本名称的后缀，一般使用的不多。


* zipAlignEnabled：表示是否对APK包执行ZIP对齐优化，减小zip体积，增加运行效率，release和debug默认都为true。

#### sourceSets{}闭包

配置目录指向
```java
    sourceSets {//目录指向配置
        main {
            jniLibs.srcDirs = ['libs']//指定lib库目录
        }
    }

```
配置 jniLibs.srcDirs = ['libs']，可以在Android studio的Android视图下生成jniLibs文件夹，可以方便我们存放jar包和库文件，其中Android视图下的jniLibs和project视图下的libs指向同一文件夹（app→libs）

#### packagingOptions{}闭包
打包时的相关配置.当项目中依赖的第三方库越来越多时，有可能会出现两个依赖库中存在同一个（名称）文件。如果这样，Gradle在打包时就会提示错误（警告）。那么就可以根据提示，然后使用以下方法将重复的文件剔除，比较常用的是通过exclude去除重复的文件，
```java
    packagingOptions{
        //pickFirsts做用是 当有重复文件时 打包会报错 这样配置会使用第一个匹配的文件打包进入apk
        // 表示当apk中有重复的META-INF目录下有重复的LICENSE文件时  只用第一个 这样打包就不会报错
        pickFirsts = ['META-INF/LICENSE']

        //merges何必 当出现重复文件时 合并重复的文件 然后打包入apk
        //这个是有默认值得 merges = [] 这样会把默默认值去掉  所以我们用下面这种方式 在默认值后添加
        merge 'META-INF/LICENSE'

        //这个是在同时使用butterknife、dagger2做的一个处理。同理，遇到类似的问题，只要根据gradle的提示，做类似处理即可。
        exclude 'META-INF/services/javax.annotation.processing.Processor'
    }

```

#### productFlavors{}闭包 多个渠道配置
这个配置是经常会使用到的，通常在适配多个渠道的时候，需要为特定的渠道做部分特殊的处理，比如设置不同的包名、应用名等。场景：当我们使用友盟统计时，通常需要设置一个渠道ID，那么我们就可以利用productFlavors来生成对应渠道信息的包
```java
android {  
    productFlavors {
        wandoujia {
            //豌豆荚渠道包配置
            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "wandoujia"]
            //manifestPlaceholders的使用在后续章节（AndroidManifest里的占位符）中介绍
        }
        xiaomi {
            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "xiaomi"]
            applicationId "com.wiky.gradle.xiaomi" //配置包名

        }
        _360 {
            manifestPlaceholders = [UMENG_CHANNEL_VALUE: "_360"]
        }
        //...
    }  
}

```
或者用更简洁的方式
```java
android {  
    productFlavors {
        wandoujia {}
        xiaomi {}
        _360 {}
       //...
    }  

    productFlavors.all { 
        //批量修改，类似一个循序遍历
        flavor -> flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name] 
    }
}

```
配置完之后，在命令行窗口中（Terminal）中输入gradlew assembleRelease（windows）即可开始打包，在Mac系统中对应指令应该是./gradlew assembleRelease。当然，如果想要debug版本的包，将指令中assembleRelease改为assembleDebug即可。最后生成的包还是在app/build/outputs/apk中，默认命名格式如app-wandoujia-release-unsigned.apk，在module的Build Variants中可以选择相应的渠道。
注：Android Studio3.0需在主app的build.gradle里面的
defaultConfig {
targetSdkVersion：***
minSdkVersion ：***
versionCode：***
versionName ：***
//版本名后面添加一句话，意思就是flavor dimension 它的维度就是该版本号，这样维度就是都是统一的了
flavorDimensions "versionCode"
}

#### lintOptions{}闭包 代码扫描分析

Lint 是Android Studio 提供的 代码扫描分析工具，它可以帮助我们发现代码结构/质量问题，同时提供一些解决方案，而且这个过程不需要我们手写测试用例。

Lint 发现的每个问题都有描述信息和等级（和测试发现 bug 很相似），我们可以很方便地定位问题，同时按照严重程度进行解决。
```java
    //程序在编译的时候会检查lint，有任何错误提示会停止build，我们可以关闭这个开关
    lintOptions {
        abortOnError false //即使报错也不会停止打包
        checkReleaseBuilds false  //打包release版本的时候进行检测
    }
```

### dependencies{}闭包
该闭包定义了项目的依赖关系，一般项目都有三种依赖方式：
本地依赖

库依赖

远程依赖

本地依赖可以对本地的jar包或目录添加依赖关系，库依赖可以对项目中的库模块添加依赖关系，远程依赖可以对jcener库上的开源项目添加依赖关系。

从Android Studio3.0后compile引入库不在使用，而是通过api和implementation，api完全等同于以前的compile，用api引入的库整个项目都可以使用，

用implementation引入的库只有对应的Module能使用，其他Module不能使用，

由于之前的项目统一用compile依赖，导致的情况就是模块耦合性太高，不利于项目拆解，使用implementation之后虽然使用起来复杂了但是做到降低偶合兴提高安全性。
```java
dependencies {//项目的依赖关系
    implementation fileTree(include: ['*.jar'], dir: 'libs')//本地jar包依赖
    implementation 'com.android.support:appcompat-v7:27.1.1'//远程依赖
    implementation 'com.android.support.constraint:constraint-layout:1.1.2'
    testImplementation 'junit:junit:4.12'//声明测试用例库
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}

```
* implementation fileTree(include: ['*.jar'], dir: 'libs')：implementation fileTree是一个本地依赖声明，表示将libs目录下所有.jar后缀的文件都添加到项目的构建路径当中。


* implementation 'com.android.support:appcompat-v7:27.1.1'：implementation语句为 远程依赖声明，'com.android.support:appcompat-v7:27.1.1'为一个标准的远程依赖库格式，其中com.android.support为域名部分，用于区分不同公司的库；appcompat-v7为组件名称，用于区分同一个公司的不同库；27.1.1为版本号，用于区分同一个库的不同版本。加上这句声明后，Gradle在构建项目时会先检查一下本地是否已经缓存过该库，若没有缓存则自动联网下载，下载后自动添加到项目的构建路径中去。


* testImplementation和androidTestImplementation：表示声明测试用例库。

### 一个完整的build.gradle的示例
```java
// 声明是Android程序，
//com.android.application 表示这是一个应用程序模块
//com.android.library 标识这是一个库模块
//而这区别：前者可以直接运行，后着是依附别的应用程序运行
apply plugin: 'com.android.application'

android {
    signingConfigs {// 自动化打包配置
        release {// 线上环境
            keyAlias 'test'
            keyPassword '123456'
            storeFile file('test.jks')
            storePassword '123456'
        }
        debug {// 开发环境
            keyAlias 'test'
            keyPassword '123456'
            storeFile file('test.jks')
            storePassword '123456'
        }
    }
    compileSdkVersion 27//设置编译时用的Android版本
    defaultConfig {
        applicationId "com.billy.myapplication"//项目的包名
        minSdkVersion 16//项目最低兼容的版本
        targetSdkVersion 27//项目的目标版本
        versionCode 1//版本号
        versionName "1.0"//版本名称
        flavorDimensions "versionCode"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"//表明要使用AndroidJUnitRunner进行单元测试
    }
    buildTypes {// 生产/测试环境配置
        release {// 生产环境
            buildConfigField("boolean", "LOG_DEBUG", "false")//配置Log日志
            buildConfigField("String", "URL_PERFIX", "\"https://release.cn/\"")// 配置URL前缀
            minifyEnabled false//是否对代码进行混淆
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'//指定混淆的规则文件
            signingConfig signingConfigs.release//设置签名信息
            pseudoLocalesEnabled false//是否在APK中生成伪语言环境，帮助国际化的东西，一般使用的不多
            zipAlignEnabled true//是否对APK包执行ZIP对齐优化，减小zip体积，增加运行效率
            applicationIdSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
            versionNameSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
        }
        debug {// 测试环境
            buildConfigField("boolean", "LOG_DEBUG", "true")//配置Log日志
            buildConfigField("String", "URL_PERFIX", "\"https://test.com/\"")// 配置URL前缀
            minifyEnabled false//是否对代码进行混淆
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'//指定混淆的规则文件
            signingConfig signingConfigs.debug//设置签名信息
            debuggable false//是否支持断点调试
            jniDebuggable false//是否可以调试NDK代码
            renderscriptDebuggable false//是否开启渲染脚本就是一些c写的渲染方法
            zipAlignEnabled true//是否对APK包执行ZIP对齐优化，减小zip体积，增加运行效率
            pseudoLocalesEnabled false//是否在APK中生成伪语言环境，帮助国际化的东西，一般使用的不多
            applicationIdSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
            versionNameSuffix 'test'//在applicationId 中添加了一个后缀，一般使用的不多
        }
    }

    sourceSets {//目录指向配置
        main {
            jniLibs.srcDirs = ['libs']//指定lib库目录
        }
    }

    packagingOptions{//打包时的相关配置
        //pickFirsts做用是 当有重复文件时 打包会报错 这样配置会使用第一个匹配的文件打包进入apk
        // 表示当apk中有重复的META-INF目录下有重复的LICENSE文件时  只用第一个 这样打包就不会报错
        pickFirsts = ['META-INF/LICENSE']

        //merges何必 当出现重复文件时 合并重复的文件 然后打包入apk
        //这个是有默认值得 merges = [] 这样会把默默认值去掉  所以我们用下面这种方式 在默认值后添加
        merge 'META-INF/LICENSE'

        //这个是在同时使用butterknife、dagger2做的一个处理。同理，遇到类似的问题，只要根据gradle的提示，做类似处理即可。
        exclude 'META-INF/services/javax.annotation.processing.Processor'
    }

    productFlavors {
        wandoujia {}
        xiaomi {}
        _360 {}
    }

    productFlavors.all {
            //批量修改，类似一个循序遍历
        flavor -> flavor.manifestPlaceholders = [IFLYTEK_CHANNEL: name]
    }

    //程序在编译的时候会检查lint，有任何错误提示会停止build，我们可以关闭这个开关
    lintOptions {
        abortOnError false
        //即使报错也不会停止打包
        checkReleaseBuilds false
        //打包release版本的时候进行检测
    }

}

dependencies {
    //项目的依赖关系
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    //本地jar包依赖
    implementation 'com.android.support:appcompat-v7:27.1.1'
    //远程依赖
    implementation 'com.android.support.constraint:constraint-layout:1.1.2'
    testImplementation 'junit:junit:4.12'
    //声明测试用例库
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}



```

























