# WebView

## 1、简介

WebView是一个基于webkit引擎、展现web页面的控件。

Android的Webview在低版本和高版本采用了不同的webkit版本内核，4.4后直接使用了Chrome。

## 2. 作用

WebView控件功能强大，除了具有一般View的属性和设置外，还可以对url请求、页面加载、渲染、页面交互进行强大的处理。

- 显示和渲染Web页面
 
- 直接使用html文件（网络上或本地assets中）作布局

- 可和JavaScript交互调用

## 3、WebView的基本使用方法

* step1：在android.manifest中添加网络权限申请：

```xml
<!-- 添加网络权限 -->
<uses-permission android:name="android.permission.INTERNET" />
```

* step2: 在布局文件中加入一个WebView

```xml
<WebView
         android:id="@+id/mWebView"
         android:layout_width="match_parent"
         android:layout_height="match_parent" />
```
* step3: 在activity或fragment中添加代码：


```java
 val settings = mWebView.settings
        settings.allowContentAccess = true
        settings.domStorageEnabled = true
        settings.allowFileAccess = true
        settings.javaScriptEnabled = true

        mWebView.addJavascriptInterface(this, "wan")

        mWebView.webChromeClient = object : WebChromeClient() {

        }

        mWebView.webViewClient = object : WebViewClient() {
            override fun onReceivedSslError(
                view: WebView?,
                handler: SslErrorHandler?,
                error: SslError?
            ) {
                handler?.proceed()
            }

            override fun shouldOverrideUrlLoading(
                view: WebView?,
                request: WebResourceRequest?
            ): Boolean {
                Log.d("","")
                return super.shouldOverrideUrlLoading(view, request)
            }
        }

        mWebView.loadUrl(url)

```

## 4、WebView的使用方法详解

### 4.1 WebView
 * webView的状态

```java
//激活WebView为活跃状态，能正常执行网页的响应
webView.onResume() ；


//当页面被失去焦点被切换到后台不可见状态，需要执行onPause()
//通过onPause()动作通知内核暂停所有的动作，比如DOM的解析、JavaScript执行等

webView.onPause()；

 

//当应用程序(存在webview)被切换到后台时，这个方法不仅仅针对当前的webview而是全局的全应用程序的webview
 //它会暂停所有webview的布局显示、解析、延时，从而降低CPU功耗
webView.pauseTimers()


//恢复pauseTimers状态
webView.resumeTimers()；

 
//销毁Webview //在关闭了Activity时，如果Webview的音乐或视频，还在播放，就必须销毁Webview。
//但是注意：webview调用destory时,webview仍绑定在Activity上
//这是由于自定义webview构建时传入了该Activity的context对象
//因此需要先从父容器中移除webview,然后再销毁webview

rootLayout.removeView(webView);
webView.destroy();
```
 

* 前进、后退网页

```java

//是否可以后退
Webview.canGoBack()

 
//后退网页
Webview.goBack()

 
//是否可以前进
Webview.canGoForward()

//前进网页
Webview.goForward()


//以当前的index为起始点前进或者后退到历史记录中指定的steps
//如果steps为负数则为后退，正数则为前进
Webview.goBackOrForward(intsteps)


常见用法：Back键控制网页后退
问题：当不做任何处理时，浏览网页时，点击系统的“Back”键，则使用WebView的整个Browser浏览器是会直接调用finish()而结束自身而关闭当前Activity并返回到Home屏幕的（即直接出浏览器）；
那么，在这里，我们可以做一些处理，让点击“Back”键后，让网页返回上一页而不是直接退出浏览器。
此时，我们就可以在当前Activity中处理该Back 事件，如下：
public boolean onKeyDown(int keyCode, KeyEvent event) {
    if ((keyCode == KEYCODE_BACK) && mWebView.canGoBack()) {
         mWebView.goBack();
         return true;
    }
    return super.onKeyDown(keyCode, event);
}
```
 

* 清除缓存

```java
//清除网页访问留下的缓存
//由于内核缓存是全局的因此这个方法不仅仅针对webview而是针对整个应用程序.

Webview.clearCache(true);

 
//清除当前webview访问的历史记录
//只会webview访问历史记录里的所有记录除了当前访问记录
Webview.clearHistory()；

 
//这个api仅仅清除自动完成填充的表单数据，并不会清除WebView存储到本地的数据
Webview.clearFormData()；
```
 

### 4.2 WebView常用的子类--WebSettings类
 
主要功能是对WebView进行配置和管理；


* 第一步：添加访问网络权限（AndroidManifest.xml）

<uses-permission android:name="android.permission.INTERNET"/>

* 第二步：生成一个WebView组件（有两种方式）

方式1：直接在在Activity中生成

WebView webView = new WebView(this);

 
方法2：在Activity的layout文件里添加webview控件

WebView webview = (WebView) findViewById(R.id.webView);

* 第三步：进行配置-利用WebSettings子类（常见方法）

```java

//声明WebSettings子类

WebSettings webSettings = webView.getSettings();

 
//如果访问的页面中要与Javascript交互，则webview必须设置支持Javascript
webSettings.setJavaScriptEnabled(true);

 
//支持插件
webSettings.setPluginsEnabled(true);

 //设置自适应屏幕，两者合用
webSettings.setUseWideViewPort(true); //将图片调整到适合webview的大小
webSettings.setLoadWithOverviewMode(true); // 缩放至屏幕的大小

//缩放操作
webSettings.setSupportZoom(true); //支持缩放，默认为true。是下面那个的前提。
webSettings.setBuiltInZoomControls(true); //设置内置的缩放控件。若为false，则该WebView不可缩放
webSettings.setDisplayZoomControls(false); //隐藏原生的缩放控件

 
//其他细节操作
webSettings.setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK); //关闭webview中缓存
webSettings.setAllowFileAccess(true); //设置可以访问文件
webSettings.setJavaScriptCanOpenWindowsAutomatically(true); //支持通过JS打开新窗口
webSettings.setLoadsImagesAutomatically(true); //支持自动加载图片
webSettings.setDefaultTextEncodingName("utf-8");//设置编码格式


常见方法：设置WebView缓存
//优先使用缓存

WebView.getSettings().setCacheMode(WebSettings.LOAD_CACHE_ELSE_NETWORK);
//缓存模式如下：

    //LOAD_CACHE_ONLY: 不使用网络，只读取本地缓存数据

    //LOAD_DEFAULT: （默认）根据cache-control决定是否从网络上取数据。

    //LOAD_NO_CACHE: 不使用缓存，只从网络获取数据.

    //LOAD_CACHE_ELSE_NETWORK，只要本地有，无论是否过期，或者no-cache，都使用缓存中的数据

//不使用缓存
WebView.getSettings().setCacheMode(WebSettings.LOAD_NO_CACHE);
```
 

### 4.3 WebViewClient类
作用是处理各种通知和请求事件

常见方法：

shouldOverrideUrlLoading()
作用：打开网页时，不调用系统浏览器进行打开，而是在本WebView中直接显示。

//Webview控件

Webview webview = (WebView) findViewById(R.id.webView);

 
//加载一个网页
webView.loadUrl("http://www.google.com/");

//重写shouldOverrideUrlLoading()方法，使得打开网页时不调用系统浏览器， 而是在本WebView中显示

webView.setWebViewClient(new WebViewClient(){
     @Override

     public boolean shouldOverrideUrlLoading(WebView view, String url) {
           view.loadUrl(url);

           return true;

      }

});

 

onPageStarted()
作用：开始载入页面时调用此方法，在这里我们可以设定一个loading的页面，告诉用户程序正在等待网络响应。
webView.setWebViewClient(new WebViewClient(){
     @Override

      public void onPageStarted(WebView view, String url, Bitmap favicon) {
           //设定加载开始的操作

     }

});

onPageFinished()
作用：在页面加载结束时调用。我们可以关闭loading 条，切换程序动作。

webView.setWebViewClient(new WebViewClient(){
     @Override

     public void onPageFinished(WebView view, String url) {
           //设定加载结束的操作

     }

});


onLoadResource()
作用：在加载页面资源时会调用，每一个资源（比如图片）的加载都会调用一次。

webView.setWebViewClient(new WebViewClient(){
     @Override

     public boolean onLoadResource(WebView view, String url) {
           //设定加载资源的操作

     }

 });


onReceivedError()
作用：加载页面的服务器出现错误时（如404）调用。

App里面使用webview控件的时候遇到了诸如404这类的错误的时候，若也显示浏览器里面的那种错误提示页面就显得很丑陋了，那么这个时候我们的app就需要加载一个本地的错误提示页面，即webview如何加载一个本地的页面

//步骤1：写一个html文件（error_handle.html），用于出错时展示给用户看的提示页面

//步骤2：将该html文件放置到代码根目录的assets文件夹下

//步骤3：复写WebViewClient的onRecievedError方法 //该方法传回了错误码，根据错误类型可以进行不同的错误分类处理

webView.setWebViewClient(new WebViewClient(){
     @Override

     public void onReceivedError(WebView view, int errorCode, String description, String failingUrl){
           switch(errorCode) {
               case HttpStatus.SC_NOT_FOUND:

                    view.loadUrl("file:///android_assets/error_handle.html");

                    break;

           }

     }

});

 

onReceivedSslError()
作用：处理https请求 

webView默认是不处理https请求的，页面显示空白

webView.setWebViewClient(new WebViewClient() {
     @Override

     public void onReceivedSslError(WebView view, SslErrorHandler handler, SslError error) {
          handler.proceed(); //表示等待证书响应

          // handler.cancel(); //表示挂起连接，为默认方式

          // handler.handleMessage(null); //可做其他处理

      }

});

 

### 4.3 WebChromeClient类

作用是辅助 WebView 处理 Javascript 的对话框,网站图标,网站标题等等。

常见方法：

onProgressChanged()
作用：获得网页的加载进度并显示

webview.setWebChromeClient(new WebChromeClient(){
     @Override

     public void onProgressChanged(WebView view, int newProgress) {
          if (newProgress < 100) {
                String progress = newProgress + "%";

                progress.setText(progress);

          } else {
                progress.setText(“100%”);

           }

});

 
onReceivedTitle()
作用：获取Web页中的标题

 每个网页的页面都有一个标题，比如www.baidu.com这个页面的标题即“百度一下，你就知道”，那么如何知道当前webview正在加载的页面的title并进行设置呢？

webview.setWebChromeClient(new WebChromeClient(){
     @Override

      public void onReceivedTitle(WebView view, String title) {
      titleview.setText(title)；

 }

 
### 4.3 如何避免WebView内存泄露?

建议不要在xml布局文件中getApplicationgContext()定义Webview控件 ，而是在需要的时候在Activity中直接创建，并且Context上下文对象推荐使用

LinearLayout.LayoutParams params = new LinearLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT);

webView = new WebView(getApplicationContext());

webView.setLayoutParams(params);

mLayout.addView(webView);

在Activity销毁（WebView）时，先让WebView加载null内容，然后移除WebView，再销毁WebView，最后把WebView设置为null。

@Override

protected void onDestroy() {
    if (webView != null) {
        webView.loadDataWithBaseURL(null, "", "text/html", "utf-8", null);

        webView.clearHistory(); ((ViewGroup)

        webView.getParent()).removeView(mWebView);

        webView.destroy();

        webView = null;

    }

    super.onDestroy();
}

 




