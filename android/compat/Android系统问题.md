# Config.gradle

使用Config.gradle来集中管理应用程序的版本、编译、依赖项。方便之处在于当有多个模块使用同一个依赖项时，如果依赖项版本更新或者发生改变，只需要更改config文件即可。统一升级，统一管理

## 1、在工程的根目录下创建config.gradle文件

## 2、config.gradle中包含的内容
* app的编译版本信息，app的发布版本信息

```gradle
ext {
    android = [
            compileSdkVersion                  : 27,
            buildToolsVersion                  : "26.0.2",
            minSdkVersion                      : 21,
            targetSdkVersion                   : 27,
            versionCode                        : 1,
            versionName                        : "1.0"
    ]
    ......
}

```

* 各个模块使用到的依赖项的名称和版本号信息
```json
ext {
   
    version = [
            //androidx
            androidxAppcompatVersion : "1.2.0",
            androidxConstraintLayoutVersion : "2.0.4",

            //material
            androidMaterialVersion : "1.2.1",

            //test
            testJunitVersion: "1.1.2",
            testEspressoVersion: "3.3.0",
    ]

    dependencies = [
            //androidx
            androidxAppcompat : "androidx.appcompat:appcompat:${version.androidxAppcompatVersion}",
            androidxConstraintLayout : "androidx.constraintlayout:constraintlayout:${version.androidxConstraintLayoutVersion}",

            //material
            androidMaterial : "com.google.android.material:material:${version.androidMaterialVersion}",

            //test
            testJunit: "androidx.test.ext:junit:${version.testJunitVersion}",
            testEspresso: "androidx.test.espresso:espresso-core:${version.testEspressoVersion}",
    ]
           
}
```
## 3、将config.gradle导入到工程的build.gradle文件中。
```java
apply from: "config.gradle" //引用自定义config.gradle，使用config中的配置管理所有module中的配置.
```

## 4、在各个模块中使用config.gradle中定义的变量
```java
apply plugin: 'com.android.application'
apply plugin: 'com.jakewharton.butterknife'
 
android {
    compileSdkVersion rootProject.ext.android["compileSdkVersion"]
    buildToolsVersion rootProject.ext.android["buildToolsVersion"]
    defaultConfig {
        applicationId "com.example.wcystart.wanandroid"
        minSdkVersion rootProject.ext.android["minSdkVersion"]
        targetSdkVersion rootProject.ext.android["targetSdkVersion"]
        versionCode rootProject.ext.android["versionCode"]
        versionName rootProject.ext.android["versionName"]
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
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
    //base
    implementation rootProject.ext.dependencies["appcompat-v7"]
    implementation rootProject.ext.dependencies["constraint-layout"]
    implementation rootProject.ext.dependencies["design"]
    implementation rootProject.ext.dependencies["cardview-v7"]
    implementation rootProject.ext.dependencies["support-v4"]
 
    //ui
    implementation rootProject.ext.dependencies["SmartRefreshLayout"]
    implementation rootProject.ext.dependencies["SmartRefreshHeader"]
 
    implementation rootProject.ext.dependencies["banner"]
 
    //Junit test
    testImplementation rootProject.ext.dependencies["junit"]
 
    // ui test
    androidTestImplementation rootProject.ext.dependencies["runner"]
    androidTestImplementation rootProject.ext.dependencies["espresso-core"]
 
    //network
    implementation rootProject.ext.dependencies["retrofit"]
    implementation rootProject.ext.dependencies["converter-gson"]
    implementation rootProject.ext.dependencies["adapter-rxjava2"]
 
    //rx
    implementation rootProject.ext.dependencies["rxjava"]
    implementation rootProject.ext.dependencies["rxandroid"]
    implementation rootProject.ext.dependencies["rxbinding"]
    implementation rootProject.ext.dependencies["rxpermissions"]
}

```
