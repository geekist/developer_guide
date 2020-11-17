# OkGo
OkGo封装了okhttp的网络框架，可以与RxJava完美结合，比Retrofit更简单易用。支持大文件上传下载，上传进度回调，下载进度回调，表单上传（多文件和多参数一起上传），链式调用，可以自定义返回对象，支持Https和自签名证书，支持cookie自动管理，支持四种缓存模式缓存网络数据，支持301、302重定向，扩展了统一的上传管理和下载管理功能。

## 添加gradle的依赖项：
```java
//必须使用
implementation 'com.lzy.net:okgo:3.0.4'
```
下面的配置选择添加
```java
//以下三个选择添加，okrx和okrx2不能同时使用
implementation 'com.lzy.net:okrx:1.0.2'
implementation 'com.lzy.net:okrx2:2.0.2'  
implementation 'com.lzy.net:okserver:2.0.5'
```

## 在Application中初始化
```java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        //okGo网络框架初始化和全局配置
        OkHttpClient.Builder builder = new OkHttpClient.Builder();
        //builder.cookieJar(new CookieJarImpl(new SPCookieStore(this)));      //使用sp保持cookie，如果cookie不过期，则一直有效
        builder.cookieJar(new CookieJarImpl(new DBCookieStore(this)));//使用数据库保持cookie，如果cookie不过期，则一直有效
        //builder.cookieJar(new CookieJarImpl(new MemoryCookieStore()));      //使用内存保持cookie，app退出后，cookie消失
        //设置请求头
        HttpHeaders headers = new HttpHeaders();
        headers.put("commonHeaderKey1", "commonHeaderValue1");    //header不支持中文，不允许有特殊字符
        headers.put("commonHeaderKey2", "commonHeaderValue2");
        //设置请求参数
        HttpParams params = new HttpParams();
        params.put("commonParamsKey1", "commonParamsValue1");     //param支持中文,直接传,不要自己编码
        params.put("commonParamsKey2", "这里支持中文参数");
        OkGo.getInstance().init(this)                           //必须调用初始化
                .setOkHttpClient(builder.build())               //建议设置OkHttpClient，不设置会使用默认的
                .setCacheMode(CacheMode.NO_CACHE)               //全局统一缓存模式，默认不使用缓存，可以不传
                .setCacheTime(CacheEntity.CACHE_NEVER_EXPIRE)   //全局统一缓存时间，默认永不过期，可以不传
                .setRetryCount(3)                               //全局统一超时重连次数，默认为三次，那么最差的情况会请求4次(一次原始请求，三次重连请求)，不需要可以设置为0
                .addCommonHeaders(headers)                      //全局公共头
                .addCommonParams(params);
    }
}


```

## OkGo使用示例

* 基本的get请求
```java
OkGo.get(Urls.URL_METHOD)     // 请求方式和请求url
    .tag(this)                       // 请求的 tag, 主要用于取消对应的请求
    .cacheKey("cacheKey")            // 设置当前请求的缓存key,建议每个不同功能的请求设置一个
    .cacheMode(CacheMode.DEFAULT)    // 缓存模式，详细请看缓存介绍
    .execute(new StringCallback() {
        @Override
        public void onSuccess(String s, Call call, Response response) {
            // s 即为所需要的结果
        }
    });
```

* 请求bitmap

```java
OkGo.get(Urls.URL_IMAGE)//
    .tag(this)//
    .execute(new BitmapCallback() {
        @Override
        public void onSuccess(Bitmap bitmap, Call call, Response response) {
            // bitmap 即为返回的图片数据
        }
    });
```

* 普通的post请求--string文本上传

一般此种用法用于与服务器约定的数据格式，当使用该方法时，params中的参数设置是无效的，所有参数均需要通过需要上传的文本中指定，此外，额外指定的header参数仍然保持有效。

```java
OkGo.post(Urls.URL_TEXT_UPLOAD)//
    .tag(this)//
    .upString("这是要上传的长文本数据！")//
    .execute(new StringCallback() {
        @Override
        public void onSuccess(String s, Call call, Response response) {
            //上传成功
        }

        @Override
        public void upProgress(long currentSize, long totalSize, float progress, long networkSpeed) {
            //这里回调上传进度(该回调在主线程,可以直接更新ui)
        }
    });

```

* 普通的post请求---json

该方法与postString没有本质区别，只是数据格式是json,一般来说，需要自己创建一个实体bean或者一个map，把需要的参数设置进去，然后通过三方的Gson或者fastjson转换成json字符串，最后直接使用该方法提交到服务器。

```java
HashMap<String, String> params = new HashMap<>();
params.put("key1", "value1");
params.put("key2", "这里是需要提交的json格式数据");
params.put("key3", "也可以使用三方工具将对象转成json字符串");
params.put("key4", "其实你怎么高兴怎么写都行");
JSONObject jsonObject = new JSONObject(params);

OkGo.post(Urls.URL_TEXT_UPLOAD)//
    .tag(this)//
    .upJson(jsonObject.toString())//
    .execute(new StringCallback() {
        @Override
        public void onSuccess(String s, Call call, Response response) {
            //上传成功
        }

        @Override
        public void upProgress(long currentSize, long totalSize, float progress, long networkSpeed) {
            //这里回调上传进度(该回调在主线程,可以直接更新ui)
        }
    });
```

* 文件下载

```java
OkGo.<File>get(url)
        .tag(this)//
        .headers("header1", "headerValue1")//
        .params("param1", "paramValue1")//
        .execute(new FileCallback("OkGo.apk") {

            @Override
            public void onStart(Request<File, ? extends Request> request) {
                btnFileDownload.setText("正在下载中");
            }

            @Override
            public void onSuccess(Response<File> response) {
                btnFileDownload.setText("下载完成"+response.body());
            }

            @Override
            public void onError(Response<File> response) {
                btnFileDownload.setText("下载出错");
            }

            @Override
            public void downloadProgress(Progress progress) {
                System.out.println(progress);
            }
        });
```

* 在onDestroy主动销毁请求

```java
protected void onDestroy() {
        super.onDestroy();
        //取消指定tag的请求
        OkGo.getInstance().cancelTag(this);
        //取消全部请求
        OkGo.getInstance().cancelAll();
        //取消OkHttpClient的全部请求
        OkGo.cancelAll(new OkHttpClient());
        OkGo.cancelTag(new OkHttpClient(), "且指定tag");
    }
```

* 所有配置讲解

```java

OkGo.get(Urls.URL_METHOD) // 请求方式和请求url, get请求不需要拼接参数，支持get，post，put，delete，head，options请求
    .tag(this)               // 请求的 tag, 主要用于取消对应的请求
    .connTimeOut(10000)      // 设置当前请求的连接超时时间
    .readTimeOut(10000)      // 设置当前请求的读取超时时间
    .writeTimeOut(10000)     // 设置当前请求的写入超时时间
    .cacheKey("cacheKey")    // 设置当前请求的缓存key,建议每个不同功能的请求设置一个
    .cacheTime(5000)         // 缓存的过期时间,单位毫秒
    .cacheMode(CacheMode.FIRST_CACHE_THEN_REQUEST) // 缓存模式，详细请看第四部分，缓存介绍
    .setCertificates(getAssets().open("srca.cer")) // 自签名https的证书，可变参数，可以设置多个
    .addInterceptor(interceptor)            // 添加自定义拦截器
    .headers("header1", "headerValue1")     // 添加请求头参数
    .headers("header2", "headerValue2")     // 支持多请求头参数同时添加
    .params("param1", "paramValue1")        // 添加请求参数
    .params("param2", "paramValue2")        // 支持多请求参数同时添加
    .params("file1", new File("filepath1")) // 可以添加文件上传
    .params("file2", new File("filepath2")) // 支持多文件同时添加上传
    .addUrlParams("key", List<String> values) //这里支持一个key传多个参数
    .addFileParams("key", List<File> files) //这里支持一个key传多个文件
    .addFileWrapperParams("key", List<HttpParams.FileWrapper> fileWrappers)//这里支持一个key传多个文件
    .addCookie("aaa", "bbb")    // 这里可以传递自己想传的Cookie
    .addCookie(cookie)          // 可以自己构建cookie
    .addCookies(cookies)        // 可以一次传递批量的cookie
     //这里给出的泛型为 ServerModel，同时传递一个泛型的 class对象，即可自动将数据结果转成对象返回
    .execute(new DialogCallback<ServerModel>(this) {
        @Override
        public void onBefore(BaseRequest request) {
            // UI线程 请求网络之前调用
            // 可以显示对话框，添加/修改/移除 请求参数
        }

        @Override
        public ServerModel convertSuccess(Response response) throws Exception{
            // 子线程，可以做耗时操作
            // 根据传递进来的 response 对象，把数据解析成需要的 ServerModel 类型并返回
            // 可以根据自己的需要，抛出异常，在onError中处理
            return null;
        }

        @Override
        public void parseError(Call call, IOException e) {
            // 子线程，可以做耗时操作
            // 用于网络错误时在子线程中执行数据耗时操作,子类可以根据自己的需要重写此方法
        }

        @Override
        public void onSuccess(ServerModel serverModel, Call call, Response response) {
            // UI 线程，请求成功后回调
            // ServerModel 返回泛型约定的实体类型参数
            // call        本次网络的请求信息，如果需要查看请求头或请求参数可以从此对象获取
            // response    本次网络访问的结果对象，包含了响应头，响应码等      
        }

        @Override
        public void onCacheSuccess(ServerModel serverModel, Call call) {
            // UI 线程，缓存读取成功后回调
            // serverModel 返回泛型约定的实体类型参数
            // call        本次网络的请求信息
        }

        @Override
        public void onError(Call call, Response response, Exception e) {
            // UI 线程，请求失败后回调
            // call        本次网络的请求对象，可以根据该对象拿到 request
            // response    本次网络访问的结果对象，包含了响应头，响应码等          
            // e           本次网络访问的异常信息，如果服务器内部发生了错误，响应码为 404,或大于等于500
        }

        @Override
        public void onCacheError(Call call, Exception e) {
            // UI 线程，读取缓存失败后回调
            // call        本次网络的请求对象，可以根据该对象拿到 request
            // e           本次网络访问的异常信息，如果服务器内部发生了错误，响应码为 404,或大于等于500
        }

        @Override
        public void onAfter(ServerModel serverModel, Exception e) {
            // UI 线程，请求结束后回调，无论网络请求成功还是失败，都会调用，可以用于关闭显示对话框
            // ServerModel 返回泛型约定的实体类型参数，如果网络请求失败，该对象为　null
            // e           本次网络访问的异常信息，如果服务器内部发生了错误，响应码为 404,或大于等于500
        }

        @Override
        public void upProgress(long currentSize, long totalSize, float progress, long networkSpeed) {
            // UI 线程，文件上传过程中回调，只有请求方式包含请求体才回调（GET,HEAD不会回调）
            // currentSize  当前上传的大小（单位字节）
            // totalSize 　 需要上传的总大小（单位字节）
            // progress     当前上传的进度，范围　0.0f ~ 1.0f
            // networkSpeed 当前上传的网速（单位秒）
        }

        @Override
        public void downloadProgress(long currentSize, long totalSize, float progress, long networkSpeed) {
            // UI 线程，文件下载过程中回调
            //参数含义同　上传相同
        }
    });
```
一次普通请求所有能配置的参数，真实使用时不需要配置这么多，按自己的需要选择性的使用即可

params添加参数的时候,最后一个isReplace为可选参数,默认为true,即代表相同key的时候,后添加的会覆盖先前添加的

多文件和多参数的表单上传，同时支持进度监听

自签名网站https的访问，调用setCertificates方法即可

为单个请求设置超时，比如涉及到文件的需要设置读写等待时间多一点。

Cookie一般情况下只需要在初始化的时候调用setCookieStore即可实现cookie的自动管理，如果特殊业务需要，需要手动额外向服务器传递自定义的cookie，可以在每次请求的时候调用addCookie方法，该方法提供了3个重载形式，可以根据自己的需要选择使用。











