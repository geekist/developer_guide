# Android通过WebView与JS交互

## 一、Android通过WebView调用JS代码

### 1.1 通过WebView的loadUrl（）

**首先loadUrl加载url到webview中，然后再次loadUrl（JavaScript：：method（））**

* 首先将需要调用的JS代码以html的格式放到src/main/assets文件夹里（如果服务器文件，不需要html文件，直接将js文件路径改为url即可

```xml
// 文本名：javascript
<!DOCTYPE html>
<html>

   <head>
      <meta charset="utf-8">
      <title>Carson_Ho</title>
      
// JS代码
     <script>
// Android需要调用的方法
   function callJS(){
      alert("Android调用了JS的callJS方法");
   }
</script>

   </head>

</html>
```
* 在Android代码中通过loadUri来调用

```java
 public class MainActivity extends AppCompatActivity {

    WebView mWebView;
    Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mWebView =(WebView) findViewById(R.id.webview);

        WebSettings webSettings = mWebView.getSettings();

        // 设置与Js交互的权限
        webSettings.setJavaScriptEnabled(true);
        // 设置允许JS弹窗
        webSettings.setJavaScriptCanOpenWindowsAutomatically(true);

        // 先载入JS代码
        // 格式规定为:file:///android_asset/文件名.html
        mWebView.loadUrl("file:///android_asset/javascript.html");

        button = (Button) findViewById(R.id.button);


        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 通过Handler发送消息
                mWebView.post(new Runnable() {
                    @Override
                    public void run() {

                        // 注意调用的JS方法名要对应上
                        // 调用javascript的callJS()方法
                        mWebView.loadUrl("javascript:callJS()");
                    }
                });
                
            }
        });

        // 由于设置了弹窗检验调用结果,所以需要支持js对话框
        // webview只是载体，内容的渲染需要使用webviewChromClient类去实现
        // 通过设置WebChromeClient对象处理JavaScript的对话框
        //设置响应js 的Alert()函数
        mWebView.setWebChromeClient(new WebChromeClient() {
            @Override
            public boolean onJsAlert(WebView view, String url, String message, final JsResult result) {
                AlertDialog.Builder b = new AlertDialog.Builder(MainActivity.this);
                b.setTitle("Alert");
                b.setMessage(message);
                b.setPositiveButton(android.R.string.ok, new DialogInterface.OnClickListener() {
                    @Override
                    public void onClick(DialogInterface dialog, int which) {
                        result.confirm();
                    }
                });
                b.setCancelable(false);
                b.create().show();
                return true;
            }

        });


    }
}


```

### 1.2 通过WebView的evaluateJavscript()

* 调用方法和上面类似，只需要修改android代码，将loadUrl换成evaluateJavascript即可

```java
// 只需要将第一种方法的loadUrl()换成下面该方法即可
    mWebView.evaluateJavascript（"javascript:callJS()", new ValueCallback<String>() {
        @Override
        public void onReceiveValue(String value) {
            //此处为 js 返回的结果
        }
    });
}
```

## 二、JS通过WebView调用Android代码


### 2.1 通过 WebView的addJavascriptInterface（）进行对象映射

* 首先在android代码中定义映射文件

```java
// 继承自Object类
public class AndroidtoJs extends Object {

    // 定义JS需要调用的方法
    // 被JS调用的方法必须加入@JavascriptInterface注解
    @JavascriptInterface
    public void hello(String msg) {
        System.out.println("JS调用了Android的hello方法");
    }
}
```


* 然后在html文件中调用接口方法

```xml
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <title>Carson</title>  
      <script>
         function callAndroid(){
        // 由于对象映射，所以调用test对象等于调用Android映射的对象
            test.hello("js调用了android中的hello方法");
         }
      </script>
   </head>
   <body>
      //点击按钮则调用callAndroid函数
      <button type="button" id="button1" "callAndroid()"></button>
   </body>
</html>


```

* 最后将js和Android的调用绑定起来

```java
public class MainActivity extends AppCompatActivity {

    WebView mWebView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mWebView = (WebView) findViewById(R.id.webview);
        WebSettings webSettings = mWebView.getSettings();

        // 设置与Js交互的权限
        webSettings.setJavaScriptEnabled(true);

        // 通过addJavascriptInterface()将Java对象映射到JS对象
        //参数1：Javascript对象名
        //参数2：Java对象名
        mWebView.addJavascriptInterface(new AndroidtoJs(), "test");//AndroidtoJS类对象映射到js的test对象

        // 加载JS代码
        // 格式规定为:file:///android_asset/文件名.html
        mWebView.loadUrl("file:///android_asset/javascript.html");
```

### 2.2 通过 WebViewClient 的方法shouldOverrideUrlLoading ()回调拦截 url

Android通过 WebViewClient 的回调方法shouldOverrideUrlLoading ()拦截 url,解析该 url 的协议,如果检测到是预先约定好的协议，就调用相应方法

 * 在JS约定所需要的Url协议

```xml
<!DOCTYPE html>
<html>

   <head>
      <meta charset="utf-8">
      <title>Carson_Ho</title>
      
     <script>
         function callAndroid(){
            /*约定的url协议为：js://webview?arg1=111&arg2=222*/
            document.location = "js://webview?arg1=111&arg2=222";
         }
      </script>
</head>

<!-- 点击按钮则调用callAndroid（）方法  -->
   <body>
     <button type="button" id="button1" "callAndroid()">点击调用Android代码</button>
   </body>
</html>


```

* 在Android通过WebViewClient复写shouldOverrideUrlLoading()拦截并检查协议

```java
public class MainActivity extends AppCompatActivity {

    WebView mWebView;
//    Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mWebView = (WebView) findViewById(R.id.webview);

        WebSettings webSettings = mWebView.getSettings();

        // 设置与Js交互的权限
        webSettings.setJavaScriptEnabled(true);
        // 设置允许JS弹窗
        webSettings.setJavaScriptCanOpenWindowsAutomatically(true);

        // 步骤1：加载JS代码
        // 格式规定为:file:///android_asset/文件名.html
        mWebView.loadUrl("file:///android_asset/javascript.html");


// 复写WebViewClient类的shouldOverrideUrlLoading方法
mWebView.setWebViewClient(new WebViewClient() {
     @Override
     public boolean shouldOverrideUrlLoading(WebView view, String url) {
// 步骤2：根据协议的参数，判断是否是所需要的url
// 一般根据scheme（协议格式） & authority（协议名）判断（前两个参数）
//假定传入进来的 url = "js://webview?arg1=111&arg2=222"（同时也是约定好的需要拦截的）
Uri uri = Uri.parse(url);                                 
// 如果url的协议 = 预先约定的 js 协议
// 就解析往下解析参数
if ( uri.getScheme().equals("js")) {
// 如果 authority  = 预先约定协议里的 webview，即代表都符合约定的协议
 // 所以拦截url,下面JS开始调用Android需要的方法
if (uri.getAuthority().equals("webview")) {
//  步骤3：
// 执行JS所需要调用的逻辑
System.out.println("js调用了Android的方法");
// 可以在协议上带有参数并传递到Android上
HashMap<String, String> params = new HashMap<>();
Set<String> collection = uri.getQueryParameterNames();
}
return true;
}
return super.shouldOverrideUrlLoading(view, url);
}
 }
);
  }
}
```

### 2.3 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调分别拦截JS对话框

Android通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调分别拦截JS对话框，得到他们的消息内容，然后解析即可。

* 加载JS代码,双方约定好协议内容

```xml
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <title>Carson_Ho</title>
      
     <script>
        
	function clickprompt(){
    // 调用prompt（）
    var result=prompt("js://demo?arg1=111&arg2=222");
    alert("demo " + result);
}

      </script>
</head>

<!-- 点击按钮则调用clickprompt()  -->
   <body>
     <button type="button" id="button1" "clickprompt()">点击调用Android代码</button>
   </body>
</html>


```

* 在Android通过WebChromeClient复写onJsPrompt（）

```java
public class MainActivity extends AppCompatActivity {

    WebView mWebView;
//    Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mWebView = (WebView) findViewById(R.id.webview);

        WebSettings webSettings = mWebView.getSettings();

        // 设置与Js交互的权限
        webSettings.setJavaScriptEnabled(true);
        // 设置允许JS弹窗
        webSettings.setJavaScriptCanOpenWindowsAutomatically(true);

// 先加载JS代码
        // 格式规定为:file:///android_asset/文件名.html
        mWebView.loadUrl("file:///android_asset/javascript.html");

        mWebView.setWebChromeClient(new WebChromeClient() {
  // 拦截输入框(原理同方式2)
  // 参数message:代表promt（）的内容（不是url）
  // 参数result:代表输入框的返回值
   @Override
   public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {
   // 根据协议的参数，判断是否是所需要的url(原理同方式2)
  // 一般根据scheme（协议格式） & authority（协议名）判断（前两个参数）
  //假定传入进来的 url = "js://webview?arg1=111&arg2=222"（同时也是约定好的需要拦截的）

  Uri uri = Uri.parse(message);
  // 如果url的协议 = 预先约定的 js 协议
   // 就解析往下解析参数
   if ( uri.getScheme().equals("js")) {

    // 如果 authority  = 预先约定协议里的 webview，即代表都符合约定的协议
    // 所以拦截url,下面JS开始调用Android需要的方法
    if (uri.getAuthority().equals("webview")) {
    // 执行JS所需要调用的逻辑
    System.out.println("js调用了Android的方法");
    // 可以在协议上带有参数并传递到Android上
    HashMap<String, String> params = new HashMap<>();
    Set<String> collection = uri.getQueryParameterNames();
//参数result:代表消息框的返回值(输入值)
     result.confirm("js调用了Android的方法成功啦");
    }
    return true;
    }
    return super.onJsPrompt(view, url, message, defaultValue, result);
    }
// 通过alert()和confirm()拦截的原理相同，此处不作过多讲述
// 拦截JS的警告框
    @Override
     public boolean onJsAlert(WebView view, String url, String message, JsResult result) {
            return super.onJsAlert(view, url, message, result);
     }

     // 拦截JS的确认框
     @Override
     public boolean onJsConfirm(WebView view, String url, String message, JsResult result) {
         return super.onJsConfirm(view, url, message, result);
     }
        }
     );
}
}

```

[CSDN的一篇比较详细的webview和js交互的文章](https://blog.csdn.net/carson_ho/article/details/64904691)
