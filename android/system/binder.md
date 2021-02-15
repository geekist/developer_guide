
# Android系统进行间通讯机制Binder介绍

关于binder需要掌握六点：

* 1、android系统-- 多用户linux系统

* 2、linux系统的进程空间--用户空间和内核空间

* 3、linux进程间通信的方法--通过内核空间：管道、信号、信号量、消息队列、共享内存、socket。。。

* 4、android的通信方法binder的特点--一次拷贝，两次映射

* 5、binder系统的组成部分---client、server、servicemanager、binder驱动

* 6、binder系统的工作流程--servicemanager注册、server add，clientget



## 一、Android应用进程空间介绍

### 1、Android应用的安全机制
每个 Android 应用都处于各自的安全沙盒中，并受以下 Android 安全功能的保护：

* Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户。

* 每个应用分配一个唯一的 Linux 用户 ID,每个ID拥有自己的进程空间、虚拟机、文件，应用通常运行在自己的进程内，其他用户无法主动访问。

默认情况下，系统会为每个应用分配一个唯一的 Linux 用户 ID（该 ID 仅由系统使用，应用并不知晓）。系统会为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；
每个进程都拥有自己的虚拟机 (VM)，因此应用代码独立于其他应用而运行。

默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 系统会在需要执行任何应用组件时启动该进程，然后当不再需要该进程或系统必须为其他应用恢复内存时，其便会关闭该进程。

Android 系统实现了最小权限原则。换言之，默认情况下，每个应用只能访问执行其工作所需的组件，而不能访问其他组件。这样便能创建非常安全的环境，在此环境中，应用无法访问其未获得权限的系统部分。

* 各个应用之间可以通过授权方式进行访问。

应用可通过一些途径与其他应用共享数据以及访问系统服务：

可以安排两个应用共享同一 Linux 用户 ID，在此情况下，二者便能访问彼此的文件。为节省系统资源，也可安排拥有相同用户 ID 的应用在同一 Linux 进程中运行，并共享同一 VM。应用还必须使用相同的证书进行签名。
应用可以请求访问设备数据（如用户的联系人、短信消息、可装载存储装置（SD 卡）、相机、蓝牙等）的权限。用户必须明确授予这些权限。如需了解详细信息，请参阅使用系统权限。

## 二、Linux系统进程间通信的原理及方法介绍

### 1、Android应用进程空间解析

一个Android进程空间由内核空间和用户空间组成。内核空间是所有进程共享的空间。用户空间是应有独有的空间。即内核空间是可以通过一定方式共享的，而用户空间是不可以共享的。

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy85NDQzNjUtMTNkNTkwNThkNGUwY2JhMS5wbmc_aW1hZ2VNb2dyMi9hdXRvLW9yaWVudC9zdHJpcCU3Q2ltYWdlVmlldzIvMi93LzEyNDA)


### 2、Linux的进程间通信机制和binder的比较

Linux的通信机制IPC有：

* 管道：pipe，

* 信号：signal，

* 跟踪：trace，

* messageQueue，

* sharedMemory，

* semaphore（信号量）等。

Android使用了Binder机制作为进程间通信的机制。和其他通信机制相比，有性能、安全和稳定方面的优势。

![](https://pic3.zhimg.com/80/v2-30dce36be4e6617596b5fab96ef904c6_720w.jpg)


## 三、Android系统的进程间通信机制--binder

### 1、binder底层实现逻辑--内存映射

一次完整的 Binder IPC 通信过程通常是这样：

* 首先 Binder 驱动在内核空间创建一个数据接收缓存区；


* 接着在内核空间开辟一块内核缓存区，建立内核缓存区和内核中数据接收缓存区之间的映射关系，以及内核中数据接收缓存区和接收进程用户空间地址的映射关系；


* 发送方进程通过系统调用 copyfromuser() 将数据 copy 到内核中的内核缓存区，由于内核缓存区和接收进程的用户空间存在内存映射，因此也就相当于把数据发送到了接收进程的用户空间，这样便完成了一次进程间的通信。

![](https://pic4.zhimg.com/80/v2-cbd7d2befbed12d4c8896f236df96dbf_720w.jpg)


### 2、binder机制架构

Android系统的Binder机制由一系统组件组成，分别是Client、Server、Service Manager和Binder驱动程序.

其中 Client、Server、Service Manager 运行在用户空间，Binder 驱动运行在内核空间。

Service Manager 和 Binder 驱动由系统提供，而 Client、Server 由应用程序来实现。

Client、Server 和 ServiceManager 均是通过系统调用 open、mmap 和 ioctl 来访问设备文件 /dev/binder，从而实现与 Binder 驱动的交互来间接的实现跨进程通信。

![](http://gityuan.com/images/binder/binder_start_service/ams_ipc.jpg)


* Binder 驱动

Binder 驱动就如同路由器一样，是整个通信的核心；驱动负责进程之间 Binder 通信的建立，Binder 在进程之间的传递，Binder 引用计数管理，数据包在进程之间的传递和交互等一系列底层支持。

* ServiceManager 与实名 Binder

ServiceManager 和 DNS 类似，作用是将字符形式的 Binder 名字转化成 Client 中对该 Binder 的引用，使得 Client 能够通过 Binder 的名字获得对 Binder 实体的引用。

* Server

Server 创建 Binder，并为它起一个字符形式，可读易记得名字，将这个 Binder 实体连同名字一起以数据包的形式通过 Binder 驱动发送给 ServiceManager ，通知 ServiceManager 注册一个名为“张三”的 Binder，它位于某个 Server 中。

binder驱动为这个穿越进程边界的 Binder 创建位于内核中的实体节点以及 ServiceManager 对实体的引用，将名字以及新建的引用打包传给 ServiceManager。ServiceManger 收到数据后从中取出名字和引用填入查找表。

* client

发起对server的请求。


ServierManager 是一个进程，Server 是另一个进程，Server 向 ServiceManager 中注册 Binder 必然涉及到进程间通信。当前实现进程间通信又要用到进程间通信，这就好像蛋可以孵出鸡的前提却是要先找只鸡下蛋！Binder 的实现比较巧妙，就是预先创造一只鸡来下蛋。ServiceManager 和其他进程同样采用 Bidner 通信，ServiceManager 是 Server 端，有自己的 Binder 实体，其他进程都是 Client，需要通过这个 Binder 的引用来实现 Binder 的注册，查询和获取。ServiceManager 提供的 Binder 比较特殊，它没有名字也不需要注册。当一个进程使用 BINDERSETCONTEXT_MGR 命令将自己注册成 ServiceManager 时 Binder 驱动会自动为它创建 Binder 实体（这就是那只预先造好的那只鸡）。其次这个 Binder 实体的引用在所有 Client 中都固定为 0 而无需通过其它手段获得。也就是说，一个 Server 想要向 ServiceManager 注册自己的 Binder 就必须通过这个 0 号引用和 ServiceManager 的 Binder 通信。类比互联网，0 号引用就好比是域名服务器的地址，你必须预先动态或者手工配置好。要注意的是，这里说的 Client 是相对于 ServiceManager 而言的，一个进程或者应用程序可能是提供服务的 Server，但对于 ServiceManager 来说它仍然是个 Client。

### 3、Binder 通信过程

#### 第一步：Service Manager注册并启动

ServiceManager进程使用 BINDERSETCONTEXT_MGR 命令通过 Binder 驱动将自己注册成为 ServiceManager；

Service Manager是成为Android进程间通信（IPC）机制Binder守护进程的过程是这样的：

* 1. 打开/dev/binder文件：open("/dev/binder", O_RDWR);
* 2. 建立128K内存映射：mmap(NULL, mapsize, PROT_READ, MAP_PRIVATE, bs->fd, 0);
* 3. 通知Binder驱动程序它是守护进程：binder_become_context_manager(bs);
* 4. 进入循环等待请求的到来：binder_loop(bs, svcmgr_handler);
在这个过程中，在Binder驱动程序中建立了一个struct binder_proc结构、一个struct  binder_thread结构和一个struct binder_node结构，这样，Service Manager就在Android系统的进程间通信机制Binder担负起守护进程的职责了。


#### 第二步：server向ServiceManager注册。

Server 通过驱动向 ServiceManager 中注册 Binder（Server 中的 Binder 实体），表明可以对外提供服务。驱动为这个 Binder 创建位于内核中的实体节点以及 ServiceManager 对实体的引用，将名字以及新建的引用打包传给 ServiceManager，ServiceManger 将其填入查找表。

通过addService将各个进程空间注册的服务搜集起来。

#### 第三步：client通过serviceManager的接口中的binder，到底层binder 驱动程序中找到server，然后响应client的请求。

Client 通过名字，在 Binder 驱动的帮助下从 ServiceManager 中获取到对 Binder 实体的引用，通过这个引用就能实现和 Server 进程的通信。

* 首先创建Service Manager的接口
```java
gDefaultServiceManager = new BpServiceManager(new BpBinder(0));
```
该接口含有一个binder的引用。

* 然后客户端client通过getServiceManager得到serviceManager，调用getService来获取service。

其实质是通过serviceManager中的binder，传递到底层的binder驱动程序，由binder驱动程序完成对service的调用。

通过其中的binder和binder驱动程序进行互动。

### 5、 Binder 通信中的代理模式

我们已经解释清楚 Client、Server 借助 Binder 驱动完成跨进程通信的实现机制了，但是还有个问题会让我们困惑。A 进程想要 B 进程中某个对象（object）是如何实现的呢？毕竟它们分属不同的进程，A 进程 没法直接使用 B 进程中的 object。

前面我们介绍过跨进程通信的过程都有 Binder 驱动的参与，因此在数据流经 Binder 驱动的时候驱动会对数据做一层转换。当 A 进程想要获取 B 进程中的 object 时，驱动并不会真的把 object 返回给 A，而是返回了一个跟 object 看起来一模一样的代理对象 objectProxy，这个 objectProxy 具有和 object 一摸一样的方法，但是这些方法并没有 B 进程中 object 对象那些方法的能力，这些方法只需要把把请求参数交给驱动即可。对于 A 进程来说和直接调用 object 中的方法是一样的。

当 Binder 驱动接收到 A 进程的消息后，发现这是个 objectProxy 就去查询自己维护的表单，一查发现这是 B 进程 object 的代理对象。于是就会去通知 B 进程调用 object 的方法，并要求 B 进程把返回结果发给自己。当驱动拿到 B 进程的返回结果后就会转发给 A 进程，一次通信就完成了。

### System Server进程和Service Manager进程
system server 进程是zygotefork出来的，用来创建各种服务。比如AMS，WM等。
service Manager进程用来管理上面创建的各种服务。