## BuildConfig与build.gradle的关系



关于BuildConfig的文件大家应该都很了解了吧，这里就简单做个简单的介绍


* BuildConfig配置文件

程序编译成功后，会根据当前的buildType生成在app\build\generated\source\buildConfig\debug(release)\包名下生成相应的文件。

内容包括：

```xml

APPLICATION_ID:当前应用的包名

FLAVOR:产品（渠道包的名称）

BUILD_TYPE:当前的编译类型(release/debug)

VERSION_CODE:版本号(数字)

VERSION_NAME:版本号

```


* BuildCongfig配置文件的应用：
 
在实际开发中其实用的最多的是BuildConfig.DEBUG，通过这个来在debug模式下进行日志输出。
不需要自己手动添加个Debug开关，这个会依据开发者的Build类型自动设定。如此，便可以利用BuildConfig.DEBUG来实现只在Debug模式下运行的代码。

```java

if(BuildConfig.DEBUG){

Log.d(TAG,"这就是个测试");

}

```

* build.gradle与defaultConfig关系


![](./build-gradle.png)


看到这里大家应该明白了，BuildConfig就是根据build.gradle配置自动生成的。

大家可以试下增加个productFlavor,修改下版本号，可以看到版本号发生变化。

其实获取应用的版本号可以通过BuildConfig.VERSION_CODE来进行获取

注：以前设置包名和版本号在AndroidManifest中设置，现在即使配置了，也不生效。现在是根据build.gradle中的
appLicationId,versionCode,versionName来生成自定义BuildConfig

可以在BuildConfig中增加一些常量，如与服务端进行请求的BASE_URL,当前运行的环境ENV。现在以添加BASE_UEL举例


在build.gradle中添加buildConfigField "String", "BASE_URL", "\"http://www.jianshu.com\""

```xml
buildTypes {
debug{
buildConfigField "String", "BASE_URL", "\"http://www.jianshu.com\""

minifyEnabled false
proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

}
release {
minifyEnabled false
proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
}
}
```

关键就在buildConfigField进行添加，上述只在debug编译环境才能获取BASE_URL。release不能获取到
	
编译下工程，debug/.../BuildConfig.java就会生成相应的代码

