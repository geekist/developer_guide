
## 一、ANR概述

ANR(Application Not responding)，是指应用程序未响应，Android系统对于一些事件需要在一定的时间范围内完成，如果超过预定时间能未能得到有效响应或者响应时间过长，都会造成ANR。一般地，这时往往会弹出一个提示框，告知用户当前xxx未响应，用户可选择继续等待或者Force Close。


## 二、造成ANR的场景

Service Timeout:比如前台服务在20s内未执行完成；

BroadcastQueue Timeout：比如前台广播在10s内未执行完成

ContentProvider Timeout：内容提供者,在publish过超时10s;

InputDispatching Timeout: 输入事件分发超时5s，包括按键和触摸事件。


可能的原因：

a. 主线程频繁进行耗时的IO操作：如数据库读写

b. 多线程操作的死锁，主线程被block；

c. 主线程被Binder 对端block；

d. SystemServer中WatchDog出现ANR；

e. servicebinder的连接达到上限无法和和System Server通信

f. 系统资源已耗尽（管道、CPU、IO） 


## 三、ANR 触发机制

ANR是一套监控Android应用响应是否及时的机制，可以把发生ANR比作是引爆炸弹，那么整个流程包含三部分组成：

埋定时炸弹：中控系统(system_server进程)启动倒计时，在规定时间内如果目标(应用进程)没有干完所有的活，则中控系统会定向炸毁(杀进程)目标。

拆炸弹：在规定的时间内干完工地的所有活，并及时向中控系统报告完成，请求解除定时炸弹，则幸免于难。

引爆炸弹：中控系统立即封装现场，抓取快照，搜集目标执行慢的罪证(traces)，便于后续的案件侦破(调试分析)，最后是炸毁目标。

## 四、触发机制详解举例

以service为例：

Service Timeout是位于”ActivityManager”线程中的AMS.MainHandler收到SERVICE_TIMEOUT_MSG消息时触发。

对于Service有两类:

对于前台服务，则超时为SERVICE_TIMEOUT = 20s；
对于后台服务，则超时为SERVICE_BACKGROUND_TIMEOUT = 200s
由变量ProcessRecord.execServicesFg来决定是否前台启动

**埋炸弹**

在Service进程attach到system_server进程的过程中会调用realStartServiceLocked()方法来埋下炸弹.

2.1.1 AS.realStartServiceLocked
[-> ActiveServices.java]

private final void realStartServiceLocked(ServiceRecord r, ProcessRecord app, boolean execInFg) throws RemoteException {
    ...
    //发送delay消息(SERVICE_TIMEOUT_MSG)，【见小节2.1.2】
    bumpServiceExecutingLocked(r, execInFg, "create");
    try {
        ...
        //最终执行服务的onCreate()方法
        app.thread.scheduleCreateService(r, r.serviceInfo,
                mAm.compatibilityInfoForPackageLocked(r.serviceInfo.applicationInfo),
                app.repProcState);
    } catch (DeadObjectException e) {
        mAm.appDiedLocked(app);
        throw e;
    } finally {
        ...
    }
}

2.1.2 AS.bumpServiceExecutingLocked
private final void bumpServiceExecutingLocked(ServiceRecord r, boolean fg, String why) {
    ... 
    scheduleServiceTimeoutLocked(r.app);
}

void scheduleServiceTimeoutLocked(ProcessRecord proc) {

    if (proc.executingServices.size() == 0 || proc.thread == null) {
        return;
    }
    long now = SystemClock.uptimeMillis();
    Message msg = mAm.mHandler.obtainMessage(
            ActivityManagerService.SERVICE_TIMEOUT_MSG);
    msg.obj = proc;
    
    //当超时后仍没有remove该SERVICE_TIMEOUT_MSG消息，则执行service Timeout流程【见2.3.1】
    mAm.mHandler.sendMessageAtTime(msg,
        proc.execServicesFg ? (now+SERVICE_TIMEOUT) : (now+ SERVICE_BACKGROUND_TIMEOUT));
}


该方法的主要工作发送delay消息(SERVICE_TIMEOUT_MSG). 炸弹已埋下, 我们并不希望炸弹被引爆, 那么就需要在炸弹爆炸之前拆除炸弹.

**拆炸弹** 

在system_server进程AS.realStartServiceLocked()调用的过程会埋下一颗炸弹, 超时没有启动完成则会爆炸. 那么什么时候会拆除这颗炸弹的引线呢? 经过Binder等层层调用进入目标进程的主线程handleCreateService()的过程.

2.2.1 AT.handleCreateService
[-> ActivityThread.java]

    private void handleCreateService(CreateServiceData data) {
        ...
        java.lang.ClassLoader cl = packageInfo.getClassLoader();
        Service service = (Service) cl.loadClass(data.info.name).newInstance();
        ...

        try {
            //创建ContextImpl对象
            ContextImpl context = ContextImpl.createAppContext(this, packageInfo);
            context.setOuterContext(service);
            //创建Application对象
            Application app = packageInfo.makeApplication(false, mInstrumentation);
            service.attach(context, this, data.info.name, data.token, app,
                    ActivityManagerNative.getDefault());
            //调用服务onCreate()方法 
            service.onCreate();
            
            //拆除炸弹引线[见小节2.2.2]
            ActivityManagerNative.getDefault().serviceDoneExecuting(
                    data.token, SERVICE_DONE_EXECUTING_ANON, 0, 0);
        } catch (Exception e) {
            ...
        }
    }
在这个过程会创建目标服务对象,以及回调onCreate()方法, 紧接再次经过多次调用回到system_server来执行serviceDoneExecuting.

2.2.2 AS.serviceDoneExecutingLocked
private void serviceDoneExecutingLocked(ServiceRecord r, boolean inDestroying, boolean finishing) {
    ...
    if (r.executeNesting <= 0) {
        if (r.app != null) {
            r.app.execServicesFg = false;
            r.app.executingServices.remove(r);
            if (r.app.executingServices.size() == 0) {
                //当前服务所在进程中没有正在执行的service
                mAm.mHandler.removeMessages(ActivityManagerService.SERVICE_TIMEOUT_MSG, r.app);
        ...
    }
    ...
}

该方法的主要工作是当service启动完成，则移除服务超时消息SERVICE_TIMEOUT_MSG。

**引爆炸弹** 

前面介绍了埋炸弹和拆炸弹的过程, 如果在炸弹倒计时结束之前成功拆卸炸弹,那么就没有爆炸的机会, 但是世事难料. 总有些极端情况下无法即时拆除炸弹,导致炸弹爆炸, 其结果就是App发生ANR. 接下来,带大家来看看炸弹爆炸的现场:

在system_server进程中有一个Handler线程, 名叫”ActivityManager”.当倒计时结束便会向该Handler线程发送 一条信息SERVICE_TIMEOUT_MSG,

2.3.1 MainHandler.handleMessage
[-> ActivityManagerService.java ::MainHandler]

final class MainHandler extends Handler {
    public void handleMessage(Message msg) {
        switch (msg.what) {
            case SERVICE_TIMEOUT_MSG: {
                ...
                //【见小节2.3.2】
                mServices.serviceTimeout((ProcessRecord)msg.obj);
            } break;
            ...
        }
        ...
    }
}
2.3.2 AS.serviceTimeout
void serviceTimeout(ProcessRecord proc) {
    String anrMessage = null;

    synchronized(mAm) {
        if (proc.executingServices.size() == 0 || proc.thread == null) {
            return;
        }
        final long now = SystemClock.uptimeMillis();
        final long maxTime =  now -
                (proc.execServicesFg ? SERVICE_TIMEOUT : SERVICE_BACKGROUND_TIMEOUT);
        ServiceRecord timeout = null;
        long nextTime = 0;
        for (int i=proc.executingServices.size()-1; i>=0; i--) {
            ServiceRecord sr = proc.executingServices.valueAt(i);
            if (sr.executingStart < maxTime) {
                timeout = sr;
                break;
            }
            if (sr.executingStart > nextTime) {
                nextTime = sr.executingStart;
            }
        }
        if (timeout != null && mAm.mLruProcesses.contains(proc)) {
            Slog.w(TAG, "Timeout executing service: " + timeout);
            StringWriter sw = new StringWriter();
            PrintWriter pw = new FastPrintWriter(sw, false, 1024);
            pw.println(timeout);
            timeout.dump(pw, " ");
            pw.close();
            mLastAnrDump = sw.toString();
            mAm.mHandler.removeCallbacks(mLastAnrDumpClearer);
            mAm.mHandler.postDelayed(mLastAnrDumpClearer, LAST_ANR_LIFETIME_DURATION_MSECS);
            anrMessage = "executing service " + timeout.shortName;
        }
    }

    if (anrMessage != null) {
        //当存在timeout的service，则执行appNotResponding
        mAm.appNotResponding(proc, null, null, false, anrMessage);
    }
}
其中anrMessage的内容为”executing service [发送超时serviceRecord信息]”;

## ANR的定位与分析方法

方法1：在logcat中分析
查找：anrin low_memory，slow_operation等关键字，分析是cpu、内存或其他相关。

方法2： 在traces文件中分析
trace路径：/data/anr/traces.txt

trace导出：adb pull/data/anr/traces.txt

最新的ANR信息在最开始部分，我们从stacktrace中即可找到出问题的具体行数。
