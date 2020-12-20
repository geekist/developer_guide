## Resource Files

### xml resource

####  xliff:g标签介绍：

* 老的用法：

```xml
<string name="welcome">
  欢迎 <xliff:g id="name">%1$s</xliff:g>, 排名 <xliff:g id="num">%2$d</xliff:g>
</string>
```
属性id可随意命名

%n$ms:输出的是字符串，n代表是第几个参数，设置m的值可以在输出之前放置空格

%n$md:输出的是整数，n代表是第几个参数，设置m的值可以在输出之前放置空格，也可以设为0m,在输出之前放置m个0

%n$mf:输出的是浮点数，n代表是第几个参数，设置m的值可以控制小数位数，如m=2.2时，输出格式为00.00

上面的例子在代码中的使用：

```java
tv_valid_time.setText(getString(R.string.welcome,"hello",123));
```

* 新的用法：

    <string name="app_name_version">%s version: %s </string>
    textViewVersion.text = resources.getString(R.string.app_name_version, appName, getAppVersionName())

###  kapt 插件支持android的注解处理

* 在Kotlin可以使用kapt插件来支持Android的注解处理。

在app的build.gradle添加插件
plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
}

如果向Annotation Processor传递参数，使用arguments{}

kapt {
    arguments {
        arg("key", "value")
    }
}

* java代码使用kapt插件

在app的build.gradle添加插件

plugins {
    id 'com.android.application'
    id 'kotlin-android'
    id 'kotlin-android-extensions'
    id 'kotlin-kapt'
}



java使用annotationProcessor 添加的依赖改为使用kapt。
例如添加dagger依赖

dependencies {
   ...
  annotationProcessor "com.google.dagger:dagger-compiler:$dagger-version"
}

改为
dependencies {
   ...
  kapt "com.google.dagger:dagger-compiler:$dagger-version"
}

如果依赖为java代码或者包含java代码，kapt会自动处理。

传递参数
如果是java参数，使用javacOptions{}

kapt {
    javacOptions {
        option("-Xmaxerrs", 500)
    }
}


* 9pitch

*mime