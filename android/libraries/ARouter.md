# ARouter

## github地址：https://github.com/alibaba/ARouter

## 1、Kotlin依赖项添加：
```gradle
plugins {
    id 'kotlin-kapt' //kotlin 注解处理器插件
}
kapt {
    arguments {
        arg("AROUTER_MODULE_NAME", project.getName())
    }
}

dependencies {
    //arouter 依赖项
    implementation 'com.alibaba:arouter-api:1.5.1'
    kapt 'com.alibaba:arouter-compiler:1.5.1'
}
```

## 2、初始化ARouter
```java
  if (BuildConfig.DEBUG) { // 这两行必须写在init之前，否则这些配置在init过程中将无效
        ARouter.openLog()     // 打印日志
        ARouter.openDebug()//开启调试模式(如果在InstantRun模式下运行，必须开启调试模式！线上版本需要关闭,否则有安全风险)
}
ARouter.init(application) // 尽可能早，推荐在Application中初始化
```

## 3、在每一个需要路由的地方添加route注解
注解的要求是以/开头，且至少有两级目录
```java
// 在支持路由的页面上添加注解(必选)
// 这里的路径需要注意的是至少需要有两级，/xx/xx
@Route(path = "/test/activity")
public class YourActivity extend Activity {
    ...
}
```
## 4、路由API的使用：
* 普通路由直接调用
```kotlin
  ARouter.getInstance()
        .build("/test/activity2")
        .navigation();
```
* 带有参数的路由调用：
```kotlin
ARouter.getInstance()
        .build("/kotlin/test")
        .withString("name", "老王")
        .withInt("age", 23)
        .navigation();
```
获取参数可以用自动注入的方式
```kotlin

@Route(path = "/test/activity")
class Test1Activity : AppCompatActivity() {
    @Autowired
    @JvmField
    var name: String? = null

    @Autowired
    @JvmField
    var age: Int? = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_test1)

        ARouter.getInstance().inject(this)  // Start auto inject.
        textView.text = "name is : $name age is : $age"
    }
}
```

* uri解析：类似于 Uri testUriMix = Uri.parse("arouter://m.aliyun.com/test/activity")也可以找到路由地址，这个不太明白

```java
Uri testUriMix = Uri.parse("arouter://m.aliyun.com/test/activity2");
ARouter.getInstance().build(testUriMix)
                    .withString("key1", "value1")
                    .navigation();
```

* 带动画的路由（略）

* 拦截器
如果需要拦截某一次路由，实现一个Interceptor的接口类来完成路由，并且在开始路由的地方设置回调
```java
  ARouter.getInstance()
                .build("/test/activity")
                .navigation(this, object : NavCallback() {
                    override fun onArrival(postcard: Postcard) {
                        Log.d("ARouter", "被拦截了danshi passed")
                    }
                    override fun onInterrupt(postcard: Postcard) {
                        Log.d("ARouter", "被拦截了")
                    }
                })
```
实现一个Interceptor的接口类
```java

@Interceptor(priority = 7)
class MyInterceptor : IInterceptor {
    var mContext: Context? = null
    
    override fun process(postcard: Postcard, callback: InterceptorCallback) {
        if ("/test/activity" == postcard.path) {
          //  callback.onContinue(postcard)
            callback.onInterrupt(null)
          //  callback.onContinue(postcard)
        } else {
            callback.onContinue(postcard)
        }
    }
    
    override fun init(context: Context) {
        mContext = context
        Log.e("testService", MyInterceptor::class.java.simpleName + " has init.")
    }
}
```

* 通过webView路由到制定的网址
```java

 ARouter.getInstance()
                        .build("/test/webview")
                        .withString("url", "file:///android_asset/scheme-test.html")
                        .navigation();
```

* 通过路由调用其他类的接口----service
```java

((HelloService) ARouter.getInstance().build("/yourservicegroupname/hello").navigation()).sayHello("mike");
ARouter.getInstance().navigation(HelloService.class).sayHello("mike");
```
在service类中实现了方法
```java
@Route(path = "/yourservicegroupname/hello")
public class HelloServiceImpl implements HelloService {
    Context mContext;

    @Override
    public void sayHello(String name) {
        Toast.makeText(mContext, "Hello " + name, Toast.LENGTH_SHORT).show();
    }

    /**
     * Do your init work in this method, it well be call when processor has been load.
     *
     * @param context ctx
     */
    @Override
    public void init(Context context) {
        mContext = context;
        Log.e("testService", HelloService.class.getName() + " has init.");
    }
}


```

## 最后，有一篇文章总结得很好： [探索Android路由框架-ARouter之基本使用](https://www.jianshu.com/p/6021f3f61fa6)











