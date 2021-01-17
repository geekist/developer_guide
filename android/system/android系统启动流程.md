# Android系统启动流程

## Android系统启动流程图


![android系统](http://qiushao.net/images/android-boot-flow.png)

## Android系统启动步骤：

Android系统启动过程由上图从下往上的一个过程是由 Boot Loader 引导开机，然后依次进入 -> Kernel -> Native -> Framework -> App，接来下简要说说每个过程。

### 1. BootLoader
板子上电后，芯片从固化在 ROM 里预设的代码(BOOT ROM)开始执行， BOOT ROM 会加载 Bootloader 到 RAM，然后把控制权交给 BootLoader。
BootLoader 并不隶属于 Android 系统，它的作用是初始化硬件设备，加载内核文件等，为 Android 系统内核启动搭建好所需的环境（可以把 Bootloader 类比成 PC 的 BIOS）。
Bootloader 是针对特定的主板与芯片的（与 CPU 及电路板的配置情况有关），因此，对于不同的设备制造商，它们的引导程序都是不同的。目前大多数系统使都是使用 uboot来修改的。
Bootloader 引导程序一般分两个阶段执行：

基本的硬件初始化，目的是为下一阶段的执行以及随后的 kernel 的执行准备好一些基本的硬件环境。这一阶段的代码通常用汇编语言编写，以达到短小精悍的目的。
初始化 Flash 设备，设置网络、内存等等，将 kernel 映像和根文件系统映像从 Flash 上读到 RAM 空间中，然后启动内核。这一阶段的代码通常用 C 语言来实现，以便于实现更复杂的功能和取得更好的代码可读性和可移植性。
实际上 Bootloader 还要根据 misc 分区的设置来决定是要正常启动系统内核还是要进入 recovery 进行系统升级，复位等工作。

### 2. Linux Kernel
Linux 内核负责初始化各种软硬件环境，加载驱动程序，挂载根文件系统(/)等，最重要的是，内核启动完成后，它会在根文件系统中寻找 ”init” 文件，然后启动 init 进程。

### 3. init 进程
init 进程是 Linux 系统中用户空间的第一个进程，进程号为1，我们可以说它是 root 进程或者所有进程的父进程。源码路径为: Android/system/core/init/。
init 进程的主要工作如下：

挂载虚拟文件系统：如 /sys、/dev、/proc
启动 property 服务
启动 SELinux
解析执行 init.rc 文件: init.rc 的内容比较复杂，干的活很多，比如文件系统的挂载(mount_all)，　各种 Native 系统服务的启动。我们常见的在 init.rc 中启动的系统服务有 servicemanager, adbd, mediaserver, zygote, bootanimation 等。我们在做系统开发的时候，也经常会创建一些 Native 服务，自然也是需要在 init.rc 里面配置启动的。关于 init.rc 的配置后续再讲解。
### 4. zygote 进程
上面提到 init 进程在解析 init.rc 时，会创建 zygote 进程，它是 Android 系统最重要的进程之一。后续 Android 中的应用进程都是由 zygote 进程 fork 出来的。因此，zygote 是 Android 系统所有应用的父进程。zygote 进程的实际执行文件并不是 zygote，而是 /system/bin/app_process。源码路径为: Android/frameworks/base/cmds/app_process/。　它会调用 frameworks/base/core/jni/AndroidRuntime.cpp　提供的接口启动 java 层的代码　frameworks/base/core/java/com/android/internal/os/ZygoteInit.java。至此，我们就进入到了 java 的世界。

zygote 的主要工作如下：

创建 java 虚拟机 AndroidRuntime
通过 AndroidRuntime 启动 ZygoteInit 进入 java 环境。
ZygoteInit　的主要工作如下：

创建 socket 服务，接受 ActivityManagerService 的应用启动请求。
加载 Android framework 中的 class、res（drawable、xml信息、strings）到内存。Android 通过在 zygote 创建的时候加载资源，生成信息链接，再有应用启动，fork 子进程和父进程共享信息，不需要重新加载，同时也共享 VM。
启动 SystemServer。
监听 socket，当有启动应用请求到达，fork 生成 App 应用进程。
zygote 进程的出现是为了能更快的启动应用。因为在 Android 中，每个应用都有对应一个虚拟机实例（VM）为应用分配不同的内存地址。如果 Android 系统为每一个应用启动不同的 VM 实例，就会消耗大量的内存以及时间。因此，更好的办法应当是通过创建一个虚拟机进程，由该 VM 进程预加载以及初始化核心库类，然后，由该 VM 进程 Fork 出其他虚拟机进程，这样就能达到代码共享、低内存占用以及最小的启动时间，而这个 VM 进程就是 zygote。

### 5. SystemServer 进程
与 Zygote 进程一样，SystemServer 进程同样是 Android 系统中最重要的进程之一。它的源码路径为: Android/frameworks/base/services/java/com/android/server/SystemServer.java。

SystemServer 的主要的作用是启动各种系统服务，比如 ActivityManagerService，PackageManagerService，WindowManagerService 以及硬件相关的 Service 等服务，我们平时熟知的各种系统服务其实都是在 SystemServer 进程中启动的，这些服务都运行在同一进程（即 SystemServer 进程）的不同线程中，而当我们的应用需要使用各种系统服务的时候其实也是通过与 SystemServer 进程通讯获取各种服务对象的句柄进而执行相应的操作的。在所有的服务启动完成后，会调用各服务的 service.systemReady(…) 来通知各对应的服务，系统已经就绪。

### 6. Launcher 的启动
Launcher 的启动比较复杂，而且不同版本的 Android 系统启动逻辑可能也不太一样，所以这里就不具体讨论，后续再专门讨论。但我们可以大概说明一下启动的策略。
我们知道　SystemServer　进程再启动的过程中会启动PackageManagerService，PackageManagerService启动后会将系统中的应用程序安装完成。SystemServer启动完所有的服务后，会调用各服务的 service.systemReady(…)。Ｌauncher　的启动逻辑就在 ActivityManagerService.systemReady() 中。

### 7. BootAnimation 退出
Launcher 启动完之后，我们还看不到 Launcher，因为被 BootAnimation 的画面挡住了。BootAnimation　的退出也比较复杂，后续再详细讨论。大概是第一个应用起来之后，其 ActivityThread 线程进入空闲状态时，会通过某种机制把 BootAnimation　给退出。这里的第一个应用自然就是 Launcher了。这样就能确保在 BootAnimation　退出后，用户看到的不是黑屏，而是我们的桌面了。

至此，Android系统总算是启动完成了，上面提到的这些步骤都是非常复杂的，每个步骤都可以单独花一篇或者几篇来讨论。现在我们只需要有一个整体的概念就行，其他的细节问题后续再慢慢研究。