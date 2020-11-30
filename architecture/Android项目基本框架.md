# Android项目基本框架

## 1、使用Config.gradle统一管理版本、依赖性及其配置

如果一个项目中有多个module，每个module下都会生成build.gradle文件，每个build.gradle文件都有自己的一套 sdk 版本和依赖，如果每个module的版本和依赖不统一的话，编译时会遇到各种各样的问题。使用config.gradle来统一管理项目下的各个module的版本号、签名、appid和依赖，能够解决上述问题，清晰的管理自己的项目工程 。

* 创建config.gradle文件

在工程根目录下创建config.gradle文件，统一所有module的sdk、依赖性及其版本。
```java
ext {
    android = [
            compileSdkVersion: 30,
            buildToolsVersion: "30.0.2",
            minSdkVersion    : 21,
            targetSdkVersion : 30,
            versionCode      : 1,
            versionName      : "1.0.1"
    ]
    version = [
            //kotlin
            androidx_core_ktx_version   : "1.3.2",
            //appcompat
            androidx_appcompat_version  : "1.2.0",
            //material
            android_material_version  : "1.2.0",
            //constraintlayout
            androidx_constraintlayout_version : "2.0.4",
            //test
            androidx_test_espresso_core_version : "3.3.0",
            androidx_test_ext : "1.1.2"
    ]

    dependencies = [
            //kotlin
            "androidx_core_ktx"               : "androidx.core:core-ktx:${version.androidx_core_ktx_version}",
            //appcompat
            "androidx_appcompat"              : "androidx.appcompat:appcompat:${version.androidx_appcompat_version}",
            //material
            "android_material"                : "com.google.android.material:material:${version.android_material_version}",
            //constraintlayout
            "androidx_constraintlayout"       : "androidx.constraintlayout:constraintlayout:${version.androidx_constraintlayout_version}",
            //test
            "junit"                           : 'junit:junit:4.+',
            "androidx_test_espresso_core"     : "androidx.test.espresso:espresso-core:${version.androidx_test_espresso_core_version}",
            "androidx_test_ext"               : "androidx.test.ext:junit:${version.androidx_test_espresso_core_version}",
    ]
}

```
* 将config.gradle文件导入build.gradle文件中。
在工程的build.gradle中加入apply from :"config.gradle",就可以将配置导入。

```java
apply from: "config.gradle"
......

```
* 在各个module的build.grdle文件中统一使用配置信息

```java
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
}

android {
    compileSdkVersion rootProject.ext.android["compileSdkVersion"]
    buildToolsVersion rootProject.ext.android["buildToolsVersion"]

    defaultConfig {
        applicationId "com.jon.mercury"
        minSdkVersion rootProject.ext.android["minSdkVersion"]
        targetSdkVersion rootProject.ext.android["targetSdkVersion"]
        versionCode rootProject.ext.android["versionCode"]
        versionName rootProject.ext.android["versionName"]

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
}

dependencies {

    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"

    implementation rootProject.ext.dependencies["androidx_core_ktx"]
    implementation rootProject.ext.dependencies["androidx_appcompat"]
    implementation rootProject.ext.dependencies["android_material"]
    implementation rootProject.ext.dependencies["androidx_constraintlayout"]

    testImplementation rootProject.ext.dependencies["junit"]
    androidTestImplementation rootProject.ext.dependencies["androidx_test_ext"]
    androidTestImplementation rootProject.ext.dependencies["androidx_test_espresso_core"]
}
```

## 2、组件化项目结构设计
* 组件化架构

将项目按功能拆分成功若干个组件，每个组件负责相应的功能，使用组件化开发，可以提高开发效率，降低维护成本，做到低耦合、高内聚，有利于编译选项的配置。组件化是一种思想，团队在使用组件化的过程中不必拘泥于形式，可以根据自己负责的项目大小和业务需求的需要制定合适的方案。下图介绍了一个简单的组件化项目结构。

![组件化结构][architecture]

[architecture]: https://github.com/geekist/developer_guide/blob/main/architecture/组件化结构.png

* 组件之间的跳转

使用ARouter库来实现组件之间的路由。

* 组件之间的通信
 
使用EventBus库实现组件之间的通信。

* 组件之间的manifest文件合并

* 组件之间library的依赖问题

* 避免第三方库的重复引入

* 组件化项目的混淆方案

组件化项目的Java代码混淆方案采用在集成模式下集中在app壳工程中混淆，各个业务组件不配置混淆文件。集成开发模式下在app壳工程中build.gradle文件的release构建类型中开启混淆属性，其他buildTypes配置方案跟普通项目保持一致，Java混淆配置文件也放置在app壳工程中，各个业务组件的混淆配置规则都应该在app壳工程中的混淆配置文件中添加和修改。

之所以不采用在每个业务组件中开启混淆的方案，是因为 组件在集成模式下都被 Gradle 构建成了 release 类型的arr包，一旦业务组件的代码被混淆，而这时候代码中又出现了bug，将很难根据日志找出导致bug的原因；另外每个业务组件中都保留一份混淆配置文件非常不便于修改和管理，这也是不推荐在业务组件的 build.gradle 文件中配置 buildTypes （构建类型）的原因。

## 3、模块中包的目录结构划分（PBF）

采用 PBF（按功能分包 Package By Feature）而不是 PBL（按层分包 Package By Layer）
可以参考谷歌的mvp Sample：**[todo-mvp][todo-mvp]**，其结构如下所示，很值得学习。

```
com
└── example
    └── android
        └── architecture
            └── blueprints
                └── todoapp
                    ├── BasePresenter.java
                    ├── BaseView.java
                    ├── addedittask
                    │   ├── AddEditTaskActivity.java
                    │   ├── AddEditTaskContract.java
                    │   ├── AddEditTaskFragment.java
                    │   └── AddEditTaskPresenter.java
                    ├── data
                    │   ├── Task.java
                    │   └── source
                    │       ├── TasksDataSource.java
                    │       ├── TasksRepository.java
                    │       ├── local
                    │       │   ├── TasksDbHelper.java
                    │       │   ├── TasksLocalDataSource.java
                    │       │   └── TasksPersistenceContract.java
                    │       └── remote
                    │           └── TasksRemoteDataSource.java
                    ├── statistics
                    │   ├── StatisticsActivity.java
                    │   ├── StatisticsContract.java
                    │   ├── StatisticsFragment.java
                    │   └── StatisticsPresenter.java
                    ├── taskdetail
                    │   ├── TaskDetailActivity.java
                    │   ├── TaskDetailContract.java
                    │   ├── TaskDetailFragment.java
                    │   └── TaskDetailPresenter.java
                    ├── tasks
                    │   ├── ScrollChildSwipeRefreshLayout.java
                    │   ├── TasksActivity.java
                    │   ├── TasksContract.java
                    │   ├── TasksFilterType.java
                    │   ├── TasksFragment.java
                    │   └── TasksPresenter.java
                    └── util
                        ├── ActivityUtils.java
                        ├── EspressoIdlingResource.java
                        └── SimpleCountingIdlingResource.java
```
[todo-mvp]: https://github.com/googlesamples/android-architecture/tree/todo-mvp/

## 4、MVP结构目录


## 5、MVVM结构目录



