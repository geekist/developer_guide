# Hybrid

## Hybrid 介绍
Hybird是一种混合开发应用，可以实现JS和Java代码的互通。

单纯使用ios/android原生实现，开发进度和成本受不了，而单纯使用h5/js开发，页面体验更加受不了。Hybird的目的是实现H5和Naive两者之间的权衡.

Hybrid是基于原生webview控件实现的，它主要要解决的问题有两个：原生端怎么调用JS代码和JS代码怎么调用原生端

* [webview和Javascript交互详解](https://github.com/geekist/developer_guide/blob/main/android/ui/webview_h5.md)

## JS调Android代码

* JSInterface

在Android中啊，有个叫做WebView的控件，这个控件的作用是可以在里面放一个网页然后运行它！我们前端就暂时把它理解成一个安卓APP里嵌入的微型浏览器吧这个WebView控件对象还可以调用一个方法。
一个叫webView.addJavascriptInterface(接口对象，接口名)的方法，调用后，webView控件里面的HTML页面里的JS代码，就可以调用刚才addJavascriptInterface方法里的接口对象的原生方法了！ 于是就这样，我们可以从JS间接调用原生Android代码，从此桥梁建立。

例如，比如说我们下面定一个JSInterface的类，里面的showToast方法可以弹出一个原生的Toast

Android的原生代码
```java
webView.addJavascriptInterface(new JsInterface(), "control”);
// …
public class JsInterface {
        @JavascriptInterface
        public void showToast(String toast) {
            Toast.makeText(MainActivity.this, toast, Toast.LENGTH_SHORT).show();
            log("show toast success");
        }

}
```

JS代码
```java
// WebView中的JS代码，注意control就是上面addJavascriptInterface定义的命名空间
function showToast(toast) {
  javascript:control.showToast(toast);
}
```

* JSbridge

在Android中啊，有这么一个WebChromeClient的组件，它就是上面讲到的WebView控件的一个子类。没错，它也可以在里面放一个网页去运行它，而且它牛啤的地方在于：你这个内部网页里的JS干的三件事可以被外层WebChromeClient给监听到：分别是前端的alert方法，confirm方法和prompt方法。只要你动了这三个方法，它们传递的数据就会被外部的WebChromeClient拦截和获取，这就为JS调Android的代码提供了一种方便的渠道。哎呀，三个方法这么多选哪个呢？ 一般情况下，我们会选prompt方法，因为alert方法JS相对用的比较频繁，存在起冲突的可能

* UrlRouter

这个东东还是和上面是一样的，Android的WebChromeClient控件这个类，它有个shouldOverrideUrlLoading这个方法，这个方法可以把控件内部网页的JS的Url请求给拦截了，当然了，你写在Url中的数据也同时被一并获取了。

总结：说白了JSInterface，JSBridge和UrlRouter主要的作用就是提供JS调原生代码的方式，搭一座桥梁

## Android怎么调JS代码

* web view.loadUrl

有了上面的经验你肯定知道，这事还是webview这位老哥来做的，它可以通过调用webview.loadUrl方法加载一个HTML页面，这样HTML中的JS脚本不就被调用了吗？

```xml
webView.loadUrl(“...//my.html”);
```
* webView.evaluateJavascript

上面的loadUrl有一个问题，它会导致页面刷新，而且通过加载文件的方式执行JS代码总不是我们认为最优雅的方式，我们可能期望的是执行一段指定的代码，而非一个文件，webView.evaluateJavascript就是做这件事情的，以下的代码可以执行一段JS代码

```xml
webView.evaluateJavascript（“JS代码”，Callback对象）
```
