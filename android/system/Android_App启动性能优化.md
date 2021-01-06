
# Android App 启动性能优化

## 1、启动花了多少时间？

使用ADB查看

ADB - AndroidDebugBridge。是用于连接手机和电脑的工具，可以让我们用电脑操作手机。

首先将android sdk platform-tools 目录加入系统变量

然后执行命令：

```java
D:\work\Android\MemoryAnalysis>adb shell am start -W -n 

com.ytech.memoryanalysis/.MainActivity

Status: ok

Activity: com.ytech.memoryanalysis/.MainActivity

ThisTime: 694

TotalTime: 694

WaitTime: 738

Complete
```
thisTime：am start 命令可能会启动多个Activity，如果启动多个，thisTime则是指 最后一个Activity的启动时间，如果启动的是1个，那么thisTime等于TotalTime.

TotalTime：新的应用的Activity启动的耗时。

WaitTime: AMS将当前Activity从onResume转向onPause，再启动新应用Activity的总时长，包含了TotalTime在内，所以WaitTime比TotalTime要长。

当然你也可以加上-S -R 10 ，连续启动10次，然后自己计算平均启动时长。
adb shell am start -S -R 10 -W packagename/.MainActivity

![启动命令](https://github.com/geekist/developer_guide/blob/main/android/system/starttime.png)

**第三方SDK初始化，比如Tinker,需要在欢迎页面就知道要不要合并补丁包，这个是必须的，也是耗时的，或者 极光推送SDK初始化。类似这种，采用异步线程去处理,此处建议直接new Thread去执行，而不是在启动Activity里面就用线程池，因为线程池的初始化也是要耗费时间的，还不如 new Thread去执行Runnable.异步执行的好处是，不会给UI线程带来时间上的延迟，给用户比较好的欢迎页面的体验。针对必要且耗时的任务，如果数量非常多，直接用一个线程去执行，但是启动页Activity同样会停留很久，这个时候，可以给所有任务区分一个轻重缓急，在重要任务执行完毕之后，就进入下一个Activity，不必等待非重要任务的结果**



