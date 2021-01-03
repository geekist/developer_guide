
# Android系统进行间通讯机制Binder介绍

[binder的源码分析](http://www.360doc.com/content/19/0211/14/15700426_814240466.shtml)

[binder的设计](https://blog.csdn.net/universus/article/details/6211589)

## Android系统简介

每个 Android 应用都处于各自的安全沙盒中，并受以下 Android 安全功能的保护：

Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户；

默认情况下，系统会为每个应用分配一个唯一的 Linux 用户 ID（该 ID 仅由系统使用，应用并不知晓）。系统会为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；
每个进程都拥有自己的虚拟机 (VM)，因此应用代码独立于其他应用而运行。

默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 系统会在需要执行任何应用组件时启动该进程，然后当不再需要该进程或系统必须为其他应用恢复内存时，其便会关闭该进程。

Android 系统实现了最小权限原则。换言之，默认情况下，每个应用只能访问执行其工作所需的组件，而不能访问其他组件。这样便能创建非常安全的环境，在此环境中，应用无法访问其未获得权限的系统部分。不过，应用仍可通过一些途径与其他应用共享数据以及访问系统服务：

可以安排两个应用共享同一 Linux 用户 ID，在此情况下，二者便能访问彼此的文件。为节省系统资源，也可安排拥有相同用户 ID 的应用在同一 Linux 进程中运行，并共享同一 VM。应用还必须使用相同的证书进行签名。
应用可以请求访问设备数据（如用户的联系人、短信消息、可装载存储装置（SD 卡）、相机、蓝牙等）的权限。用户必须明确授予这些权限。如需了解详细信息，请参阅使用系统权限。

## Android系统的进程间通信机制--binder
Linux的通信机制IPC有：管道：pipe，信号：signal，跟踪：trace等，messageQueue，sharedMemory，semaphore（信号量）等。

Android使用了Binder机制作为进程间通信的机制。


![android binder jizhe](http://hi.csdn.net/attachment/201107/19/0_13110996490rZN.gif)


Android系统的Binder机制由一系统组件组成，分别是Client、Server、Service Manager和Binder驱动程序.

Binder驱动程序运行内核空间。是binder机制的核心组件。

Client、Server和Service Manager运行在用户空间。

Service Manager管理Server并向Client提供查询Server远程接口的功能。

Service Manger、Client和Server三者分别是运行在独立的进程当中，这样它们之间的通信也属于进程间通信了，而且也是采用Binder机制进行进程间通信，因此，Service Manager在充当Binder机制的守护进程的角色的同时，也在充当Server的角色，然而，它是一种特殊的Server，下面我们将会看到它的特殊之处：

![binder架构](http://image109.360doc.com/DownloadImg/2019/02/1114/153964066_1_201902110252007)

 Service Manager在用户空间的源代码位于frameworks/base/cmds/servicemanager目录下，主要是由binder.h、binder.c和service_manager.c三个文件组成。Service Manager的入口位于service_manager.c文件中的main函数：
```c
int main(int argc, char **argv)
{
    struct binder_state *bs;
    void *svcmgr = BINDER_SERVICE_MANAGER;
 
    bs = binder_open(128*1024);
 
    if (binder_become_context_manager(bs)) {
        LOGE("cannot become context manager (%s)\n", strerror(errno));
        return -1;
    }
 
    svcmgr_handle = svcmgr;
    binder_loop(bs, svcmgr_handler);
    return 0;
}
```
## 第一步：启动Service Manager

Service Manager是成为Android进程间通信（IPC）机制Binder守护进程的过程是这样的：

* 1. 打开/dev/binder文件：open("/dev/binder", O_RDWR);
* 2. 建立128K内存映射：mmap(NULL, mapsize, PROT_READ, MAP_PRIVATE, bs->fd, 0);
* 3. 通知Binder驱动程序它是守护进程：binder_become_context_manager(bs);
* 4. 进入循环等待请求的到来：binder_loop(bs, svcmgr_handler);
在这个过程中，在Binder驱动程序中建立了一个struct binder_proc结构、一个struct  binder_thread结构和一个struct binder_node结构，这样，Service Manager就在Android系统的进程间通信机制Binder担负起守护进程的职责了。


## 第二步：ServiceManager搜集注册的服务。
通过addService将各个进程空间注册的服务搜集起来。

## 第三步：client通过serviceManager的接口中的binder，到底层binder 驱动程序中找到server，然后响应client的请求。

* 首先创建Service Manager的接口
```java
gDefaultServiceManager = new BpServiceManager(new BpBinder(0));
```
该接口含有一个binder的引用。

* 然后客户端client通过getServiceManager得到serviceManager，调用getService来获取service。

其实质是通过serviceManager中的binder，传递到底层的binder驱动程序，由binder驱动程序完成对service的调用。

通过其中的binder和binder驱动程序进行互动。


