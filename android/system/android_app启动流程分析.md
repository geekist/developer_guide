# AndroidApp启动分析与优化

## 一、app的启动方式

### 1、冷启动
 当启动应用时，后台没有该应用的进程，这时系统会重新创建一个新的进程分配给该应用，这个启动方式就是冷启动。

### 2、热启动
当启动应用时，后台已有该应用的进程（例：按back键、home键，应用虽然会退出，但是该应用的进程是依然会保留在后台，可进入任务列表查看），所以在已有进程的情况下，这种启动会从已有的进程中来启动应用，这个方式叫热启动。热启动因为会从已有的进程中来启动，所以热启动就不会走Application这步了，而是直接走MainActivity（包括一系列的测量、布局、绘制），所以热启动的过程只需要创建和初始化一个MainActivity就行了，而不必创建和初始化Application，因为一个应用从新进程的创建到进程的销毁，Application只会初始化一次。

### 3、温启动
温启动这个名词平时不常见到，官方文档中是这样解释的:温启动包含了冷启动的一部分操作集，同时它的消耗要比冷启动要少.温启动的常见场景如下:

- 用户退出app后重新进入(很多app会在退出时重新启动一个新的实例做到常驻，这里只讨论部不常驻的场景)。当app退出后，进程有可能仍在运行，这时候如果重新启动app,那么activity必须要从onCreate()生命周期开始重新创建.

- 系统干掉了驻留内存的app。这时如果重新启动app，那么进程和activity都是需要重新创建，但onCreate() 会传入 saveInstance,通过使用saveInstance可以节省耗时.总的来说温启动在耗时上介于冷启动和热启动之间.


## 二、根Activity（冷）启动过程

Activity的启动过程分为两种，一种是根Activity的启动过程，可以认为是初次点击桌面的应用图标，启动Manifest中注册的作为应用的启动页面的Activity。根Activity的启动过程也可以理解为应用程序的启动过程。另一种就是普通Activity的启动过程，也就是根Activity以外的Activity的启动过程。这两种Activity的启动过程有重叠的部分，但是根Activity一般理解为应用程序的启动过程，更具有指导意义。下面着重分析根Activity的启动过程进行分析。

谈到Activity的启动流程就绕不开ActivityManagerService(简称AMS)，它主要负责四大组件的启动、切换、调度以及进程的管理，是Android中最核心的服务，参与了所有应用程序的启动管理。Activity的启动流程围绕AMS，可以大致分为3个部分：

- Launcher请求AMS的过程
- AMS到ApplicationThread的调用过程
- ActivityThread启动Activity的过程

下面就针对这3个部分逐一进行分析。

### 2.1 第一部分：Launcher请求AMS的过程

该过程的时序图如下：

![](https://img-blog.csdnimg.cn/20200517153334652.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215X2NzZG5ib2tl,size_16,color_FFFFFF,t_70#pic_center)

相关类简介

Launcher: 桌面应用程序进程。

Instrumentation： 监视系统与应用程序的所有交互。

ActivityManager ：该类提供与Activity、Service和Process相关的信息以及交互方法， 可以被看作是ActivityManagerService的辅助类。

ActivityManagerService： Android中最核心的服务，主要负责系统中四大组件的启动、切换、调度及应用程序的管理和调度等工作。

当我们点击桌面的应用快捷图标时，就会调用Launcher的startActivitySafely方法：

```java
public boolean startActivitySafely(View v, Intent intent, ItemInfo item) {
    ...
    // Prepare intent
    // 注释1
    intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
    ...
            // Could be launching some bookkeeping activity
        	// 注释2
            startActivity(intent, optsBundle);
    ...
}

```
对于不太需要关注的代码省略了，主要是走调用流程，对关键代码进行分析，在注释1处将Flag设置为Intent.FLAG_ACTIVITY_NEW_TASK，这样根Activity会在新的任务栈中启动。**不同进程的activity调用必须在不同的栈中**

在注释2处调用startActivity方法，这个方法在Activity中实现，Activity中startActivity方法有好几种重载方式，但它们最终都会调用startActivityForResult方法：

```java
public void startActivityForResult(@RequiresPermission Intent intent, int requestCode,
            @Nullable Bundle options) {
    	// 注释1
        if (mParent == null) {
            options = transferSpringboardActivityOptions(options);
            // 注释2
            Instrumentation.ActivityResult ar =
                mInstrumentation.execStartActivity(
                    this, mMainThread.getApplicationThread(), mToken, this,
                    intent, requestCode, options);
            ...
        } else {
            ...
        }
    }
```

注释1处的mParent是Activity类型的，表示当前Activity的父类，因为目前根Activity还没有创建出来，因此mParent==null成立，执行注释2处的逻辑，调用Instrumentation的execStartActivity方法，Instrumentation主要是用来监控应用程序和系统的交互，execStartActivity方法代码如下：

```java
public ActivityResult execStartActivity(
            Context who, IBinder contextThread, IBinder token, Activity target,
            Intent intent, int requestCode, Bundle options) {
        ...
        try {
            intent.migrateExtraStreamToClipData();
            intent.prepareToLeaveProcess(who);
            // 注释1
            int result = ActivityManager.getService()
                .startActivity(whoThread, who.getBasePackageName(), intent,
                        intent.resolveTypeIfNeeded(who.getContentResolver()),
                        token, target != null ? target.mEmbeddedID : null,
                        requestCode, 0, null, options);
            checkStartActivityResult(result, intent);
        } catch (RemoteException e) {
            throw new RuntimeException("Failure from system", e);
        }
        return null;
    }

```
核心逻辑就是注释1的代码，首先调用ActivityManager的getService方法来获取AMS的代理对象，接着调用startActivity方法。我们先进入ActivityManager的getService方法：

```java
public static IActivityManager getService() {
        return IActivityManagerSingleton.get();
    }

private static final Singleton<IActivityManager> IActivityManagerSingleton =
        new Singleton<IActivityManager>() {
        	@Override
            protected IActivityManager create() {
                // 注释1
            	final IBinder b = ServiceManager.getService(Context.ACTIVITY_SERVICE);
                // 注释2
                final IActivityManager am = IActivityManager.Stub.asInterface(b);
                return am;
            }
 		};
getService方法调用了IActivityManagerSingleton的get方法，看一下Singleton代码

public abstract class Singleton<T> {
    private T mInstance;

    protected abstract T create();

    public final T get() {
        synchronized (this) {
            if (mInstance == null) {
                mInstance = create();
            }
            return mInstance;
        }
    }
}
```

不难发现IActivityManagerSingleton的get方法，会调用create方法，在注释1处得到IBinder类型的AMS引用，接着在注释2处将它转化成IActivityManager类型的对象，即AMS的代理对象，这段代码采用的是AIDL，IActivityManager.java类是由AIDL工具在编译时自动生成的。AMS继承IActivityManager.Stub类并实现相关方法。通过这个代理对象和AMS(AMS所在的进程为SystemServer系统服务进程)进行跨进程通信，如果你对Binder机制有一定的认识，这里就比较好理解。如果还不太熟悉Binder机制，强烈建议一定要补一下，要不然分析源码总会一知半解的。需要注意的Android8.0之前并没有采用AIDL，是用AMS的代理对象ActivityManagerProxy来与AMS进行跨进程通信的。Android8.0去除了ActivityManagerNative的内部类ActivityManagerProxy，代替它的是IActivityManager，它就是AMS的代理对象。经过上面的分析，我们知道execActivity方法最终调用的是AMS的startActivity方法。补充一句，这里就由Launcher进程经过一系列调用到了SystemServer进程，

![](https://img-blog.csdnimg.cn/20200517153450248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215X2NzZG5ib2tl,size_16,color_FFFFFF,t_70#pic_center)

这一部分的步骤如下：
* 1、launcher通过SystermServer获取ActivityManagerService的服务。

* 2、launcher向ActivityManagerService发起startActivity请求。

### 2.2 第二部分：AMS到ApplicationThread的调用过程

![](https://img-blog.csdnimg.cn/20200517153522928.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215X2NzZG5ib2tl,size_16,color_FFFFFF,t_70#pic_center=800x1000)

相关类介绍：

ActivityManagerService： Android中最核心的服务，主要负责系统中四大组件的启动、切换、调度及应用程序的管理和调度等工作。

ActivityThread：管理应用程序进程中主线程的执行，根据Activity管理者的请求调度和执行activities、broadcasts及其相关的操作。

ActivityStack：负责单个Activity栈的状态和管理。

ActivityStackSupervisor： 负责所有Activity栈的管理。内部管理了mHomeStack、mFocusedStack和mLastFocusedStack三个Activity栈。其中，mHomeStack管理的是Launcher相关的Activity栈；mFocusedStack管理的是当前显示在前台Activity的Activity栈；mLastFocusedStack管理的是上一次显示在前台Activity的Activity栈。

ClientLifecycleManager： 用来管理多个客户端生命周期执行请求的管理类。

这一部分的步骤如下：

* 3、AMS解析intent，调用packageManager判断调用权限，配置栈信息。
AMS接收到请求后通过ActivityStarter解析Intent和Flag，在通过ActivityStack为Activity配置栈信息。并判断是否需要创建应用进程。

如果需要创建新的进程：

* 4、配置应用进程信息并通过Socket请求Zygote创建子进程
通过系统进程管理类Process配置应用进程信息，这一步涉及到连个进程system_server进程和 Zygote进程，通过Socket通信。

* 5、ZygoteServer接收到AMS的Socket请求，通过ZygoteConnection fork应用进程。fork进程后会返回一个pid，通过判断pid进入到应用进程。Zygote进程在初始化的的时候就创建了Socket并Loop等待AMS的请求。

进程创建完毕或者不需要创建新的进程：

* 6、创建ActivityThread类和创建applicationThread类，启动main thread，创建Looper，Handler，启动looper的循环，向AMS发起绑定application的请求。


![](https://img-blog.csdnimg.cn/20200517155128536.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215X2NzZG5ib2tl,size_16,color_FFFFFF,t_70#pic_center)



### 2.3 第三部分：ApplicationThread启动Activity的调用过程

![](https://img-blog.csdnimg.cn/20200517155151339.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L215X2NzZG5ib2tl,size_16,color_FFFFFF,t_70#pic_center)

涉及到的类：
Instrumentation： 监视系统与应用程序的所有交互。

ActivityThread
管理应用程序进程中主线程的执行，根据Activity管理者的请求调度和执行activities、broadcasts及其相关的操作。

这一过程的步骤如下：

* 7、AMS接收到ActivityThread发送的请求后，把Application和进程进程绑定（这就是每个进程只有一个Application的原因）。最后通过Binder机制请求启动Activity。


* 8、ApplicationThread接收到请求后调用ActivityThread#performLaunchActivity来间接调用Activity的onCreate方法。这里有个注意点ActivityThread调用Activity也通过了Instrumention。

创建application实例，创建activity的实例，然后调用oncreate

## 总结
根Activity的启动过程涉及4个进程，分别是Launcher进程、AMS所在进程(SystemServer进程)、Zygote进程、应用程序进程，首先Launcher进程向AMS请求创建根Activity，AMS会判断根Activity所需要的应用程序进程是否存在并启动，如果不存储就会请求Zygote进程创建应用程序进程。应用程序进程启动后，AMS会请求应用程序进程启动根Activity。