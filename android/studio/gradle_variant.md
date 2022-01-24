


## 方法一、通过resValue设置变量，相当于向res目录下的文件中追加字段

```xml
productFlavors {
    dev {
        resValue "string", "app_name", "dev_myapp"
        resValue "bool", "isrRank", 'false'
    }
    stage {
        resValue "string", "app_name", "stage_myapp"
        resValue "bool", "isrRank", 'true'
    }
    prod {
        resValue "string", "app_name", "myapp"
        resValue "bool", "isrRank", 'true'
    }
}
```

利用 resValue 来定义资源的值，顾名思义 res 底下的内容应该都可以创建，最后用 R.xxx.xxx 来引用。
注意：这里是添加，不是覆盖，不能与res文件中已有的资源文件冲突。

## 方法二、设置manifestPlaceholders，相当于设置manifest.xml文件中的标签值

### step1：首先需要在manifest.xml中定义一个变量：

```

<application
    android:icon="@mipmap/ic_launcher"
    android:label="${k_appName}" // 这里取k_appName
>

```

### step2：在build.gradle中设置该变量的值：

```
android {
    ... ...
    defaultConfig{
        ... ...
        manifestPlaceholders = [k_appName : "哈啰"]   // 设置默认的k_appName
    }
    
    // 依据debug/release变动的话设置如下
    buildTypes {
        debug {
            manifestPlaceholders = [k_appName : "Debug哈啰"]
        }
    }
    
    
    // 依据flavors变动的话设置如下
    productFlavors {
        autoTest {
            manifestPlaceholders = [k_appName : "AT哈啰"]
        }
        
        appStore {
            // do nothing
        }
    }
}

```

## 方法三、设置buildConfigField的值，相当于设置BuildConfig文件的变量

### step1：修改模块的build.gradle文件，添加针对不同服务器的build变体，并在变体中变量。

在模块的build.gradle文件中，在android闭包中，添加一个flavor的维度：server，该维度下，定义两个变体：

dev：对应开发环境的各种网络地址配置；

prod：对应生产环境的各种网络地址配置：

```groovy

android {
    flavorDimensions "server"
    productFlavors {
        dev {
            dimension "server"
            buildConfigField "String", "BASE_URL", "\"http://47.114.170.65:9996/api\""
            buildConfigField "String", "BASE_MESSAGE_URL", "\"http://47.114.170.65:9997/api\""
            buildConfigField "String", "BASE_H5_URL", "\"http://47.114.170.65:9999\""
        }

        prod {
            dimension "server"
            buildConfigField "String", "BASE_URL", "\"http://parent.iyuya.com/api\""
            buildConfigField "String", "BASE_MESSAGE_URL", "\"http://message.iyuya.com/api\""
            buildConfigField "String", "BASE_H5_URL", "\"http://html5.iyuya.com\""
        }
    }
}

```

在上面的闭包中，我们定义好了不同环境下的网络地址变量。


### step2: 编译build变体

配置好变体后，可以看到在android studio的build variant中，已经生成好了不同的变体。我们只需要选择某一个变体，编译即可。

![](./assets/gradle_1.png)

采用上面的配置变量的方式，会在该build变体的编译过程中，生成一个名为buildConfig的java文件，我们定义的变量中会作为该类的成员变量，我们在代码中直接读取即可。

### step3：在代码中读取网络地址变量。

采用上面的配置变量的方式，会在该build变体的编译过程中，生成一个名为buildConfig的java文件，我们定义的变量中会作为该类的成员变量，我们在代码中直接读取即可。

```kotlin

import com.yuya.parent.lib.BuildConfig

object Constants {
    const val BASE_URL = BuildConfig.BASE_URL
    const val BASE_MESSAGE_URL = BuildConfig.BASE_MESSAGE_URL
    const val BASE_H5_URL = BuildConfig.BASE_H5_URL
    const val BASE_MEDIA_URL = "http://pic.iyuya.com/"
}

```