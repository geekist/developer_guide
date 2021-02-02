# Android崩溃优化

## 一、android崩溃的形式：

* 出现了 Java 或 Native 崩溃。系统重启；

* 被系统杀死。被 low memory killer 杀掉、从系统的任务管理器中划掉等。

* ANR。

## 二、Crash搜集、分析和处理

### 2.1 用UncaughtExceptionHandler接口处理Java层crash [UncaughtExceptionHandler demo](https://blog.csdn.net/qq_30379689/article/details/53731646)

主要方法如下：

1、创建BaseApplication继承Application并实现Thread.UncaughtExceptionHandler

2、通过Thread.setDefaultUncaughtExceptionHandler(this)设置默认的异常捕获

```java

public class BaseApplication extends Application implements Thread.UncaughtExceptionHandler {

    @Override
    public void onCreate() {
        super.onCreate();
        //设置异常捕获
        Thread.setDefaultUncaughtExceptionHandler(this);
    }

    @Override
    public void uncaughtException(final Thread thread, final Throwable ex) {
        //当有异常产生时执行该方法
    }
}

```

### 2.2 用BreakPad处理Native层的crash（略）[BreakPad介绍](https://juejin.cn/post/6844903746040758285)

### 2.3 用谷歌firebase、腾讯bugly，友盟、网易云捕、pgyer、crashlytics等平台分析和crash。

### 2.4 ANR分析

那些场景会出现ANR？

1）、Service Timeout:比如前台服务在20秒内未执行完成；

2）、BroadcastQueue Timeout:比如前台广播在在10秒内未执行完成；

3）、ContentProvider Timeout:内容提供者，在punlish过超时10秒；

4）、InputDispatching Timeout:输入事件分发超时5秒，如按键或者触摸事件；

参考文献：

[崩溃优化（上）：关于“崩溃”那些事儿](https://blog.yorek.xyz/android/paid/master/crash_1/)
[崩溃优化（下）：应用崩溃了，你应该如何去分析？](https://blog.yorek.xyz/android/paid/master/crash_2/)









