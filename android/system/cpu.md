

## 一、android系统的cpu架构种类


Android 系统目前支持以下七种不同的 CPU 架构：

* **armeabi/ARMv5**

第5代、第6代的ARM处理器，早期的手机用的比较多
第5代 ARM v5TE，使用软件浮点运算，兼容所有ARM设备，通用性强，速度慢（只支持armeabi）


* **ARMv7 (从2010年起)**

第7代及以上的 ARM 处理器。2011年15月以后的生产的大部分Android设备都使用它
第7代 ARM v7，使用硬件浮点运算，具有高级扩展功能（支持 armeabi 和 armeabi-v7a，目前大部分手机都是这个架构）

* **arm64-v8a** 

第8代，64位，包含AArch32、AArch64两个执行状态对应32、64bit（支持 armeabi-v7a、armeabi 和 arm64-v8a）


* **x86**

从2011年起, 平板、模拟器用得比较多

intel 32位，一般用于平板（支持 armeabi(性能有所损耗) 和 x86）

* **x86_64**

从2014年起, 64位的平板

x86_64 intel 64位，一般用于平板（支持 x86 和 x86_64）


* **MIPS (从2012年起)**

arm64-v8a: 第8代、64位ARM处理器，很少设备，三星 Galaxy S6是其中之一

（支持 mips）


* **MIPS64**

（支持 mips 和 mips_64）

## 二、ABI

* ABI. 应用程序二进制接口（Application Binary Interface）

定义了二进制文件（尤其是.so文件）如何运行在相应的系统平台上，从使用的指令集，内存对齐到可用的系统函数库。

在Android 系统上，每一个CPU架构对应一个ABI：armeabi，armeabi-v7a，x86，mips，arm64- v8a，mips64，x86_64。

## 三、.so文件如何配置

具体适配的cpu架构，在app>build.gradle文件中android->defaultConfig 里设置的支持的 ndk 为准：

```xml

ndk { 

abiFilters 'armeabi','armeabi-v7a' // , 'arm64-v8a','x86', 'x86_64' 

}

```

* lib和libs

放在lib中的是被reference的，放在libs中的是被include的。

放在libs中的文件会自动被编辑器所include。所以不要把API放到libs里去。

lib的内容是不会被打包到APK中，libs中的内容是会被打包进APK中


不同的 CPU 支持不同的指令集。当我们需要我们 APP 支持尽可能多的不同 CPU 的时候，只需要将不同版本的 .so 文件放置在不同的目录下，APK 安装运行的时候会根据自己需要而自己选取。


但是这样却带来一个问题，APK文件较大，影响用户下载。

为实现最佳性能，应直接针对主要 ABI 进行编译。例如，基于 ARMv5TE 的典型设备只会定义主要 ABI：armeabi

相反，基于 ARMv7 的典型设备将主要 ABI 定义为 armeabi-v7a，而将辅助 ABI 为 armeabi，因为它可以运行为每个 ABI 生成的应用原生二进制文件

许多基于 x86 的设备也可行 armeabi-v7a 和 armeabi NDK 二进制文件

对于这些设备，主要 ABI 将是 x86，辅助 ABI 是 armeabi-v7a

基于 MIPS 的典型设备只定义主要 ABI：mips

安装应用时，软件包管理器服务将扫描APK，查找以下形式的任何共享库：

lib/<primary-abi>/lib<name>.so

如果未找到，并且您已定义辅助 ABI，该服务将扫描以下形式的共享库：

lib/<secondary-abi>/lib<name>.so

找到所需的库时，软件包管理器会将它们复制到应用的 data 目录 (data/data/<package_name>/lib/) 下的/lib/lib<name>.so





如果适配不止一个cpu架构，比如armeabi、 armeabi-v7a 、arm64-v8a这三个，那么一定要确保三个目录中的so库数目一样；第三方库如果支持者提供这三个cpu架构的so库，那非常理想，对应放到目录就可以；
如果适配的上面三个cpu架构，第三方库只提供了两个cpu（比如armeabi、 armeabi-v7a）的库，那也要提供的armeabi的so库，复制一份（armeabi或者armeabi-v7a的so库，因为arm64-v8a兼容armeabi 和 armeabi-v7a）到没有提供的arm64-v8a这个架构目录下；如果不这么做，当应用跑到arm64-v8a架构的手机上时，找不到对应的so库就会报错
具体自己项目适配几种cpu架构，得看app性质，比如微信，主要考虑到兼容，让几乎所有手机都可以适配，另外也相对减少了apk的大小；而另外一个app，比如游戏或者一些对手机性能有要求的app，这种app就挑用户了，只适配到armeabi-v7a，因为目前主流手机都支持armeabi-v7a，就算app支持到只支持armeabi这种架构的手机，app也未必能运行的起来，体验也未必好，算是app放弃也这些用户吧，再说使用只支持armeabi这种架构的手机估计年纪也大了，也不会使用到这个app；
如果只适配一种cpu架构，armeabi（都兼容，但性能有所损耗，如微信和qq）或者armeabi-v7a（目前大部分手机都支持这种cpu架构（王者荣耀））；目前手头的app目前只支持armeabi，armeabi-v7a，但是现在apk包越来越大，后面也会考虑只支持一个cpu架构的方案（可以减少10M）;
如果app适配了armeabi、 armeabi-v7a 、arm64-v8a三种cpu架构，以我的手机mate9为例， mate9支持 armeabi、 armeabi-v7a 、arm64-v8a，那么app在找so文件时会从最新的一代的cpu 架构(arm64-v8a)找so文件,如果找不到会直接报错，不会再去armeabi-v7a 和armeabi里面找，一定要确保三个目录中的so库数目一样；如果适配armeabi、 armeabi-v7a，mate9手机上app在找so文件时会从armeabi-v7a找对应so，没有就报错；如果只适配一种，那么手机只要支持这种cpu架构，就会去这个文件夹下找对应的so，找不到就报错，如果手机不支持这种cpu架构就报错
项目中遇到的一种情况：默认情况下（不在gradle中设置ndk），某个测试机支持v8a，v7a，armeabi三种cpu架构；代码中只有armeabi和v7a两个包并且两个包内的so包完全相同；在测试机上无论debug还是release测试都不会有问题；但是后续开发中集成了某个第三方arr后，打的包就会默认在v8a中找so文件，导致程序崩溃；所以建议开发时候一定要对cpu架构做适配；仅此记录



## 四、abi的兼容性


3. CPU是向下兼容的，支持arm64-v8a，就会支持armeabi-v7a和armeabi。
若arm64-v8a文件夹中没有a.so库，armeabi-v7a文件夹有a.so库，且app-build.gradle-defaultConfig-ndk-abiFilters里没有指定ndk需要的ABI，会默认找它支持且优先级最高的ABIs
(可通过easydeviceinfo查看当前cup支持的ABIs)，若最高支持arm64-v8a，就会报错
解决方法：
1). 在build.gradle中指定ndk需要兼容的架构为armeabi-v7a
2). 在build.gradle中指定ndk需要兼容的架构为arm64-v8a，且arm64-v8a文件夹中有a.so库

4. 为了减小apk体积，只保留armeabi和armeabi-v7a两个目录，并保证这两个目录中so库数量一致。对只提供armeabi版本的第三方so库，原样复制一份到armeabi-v7a目录中。


Q1： 只适配了armeabi-v7a,那如果APP装在其他架构的手机上，如arm64-v8a上，会蹦吗？
A: 不会，但是反过来会。
因为armeabi-v7a和arm64-v8a会向下兼容：

只适配armeabi的APP可以跑在armeabi,x86,x86_64,armeabi-v7a,arm64-v8上
只适配armeabi-v7a可以运行在armeabi-v7a和arm64-v8a
只适配arm64-v8a 可以运行在arm64-v8a上



## 五、Android如何加载So库


![](./assets/find_so.png)

对于一个cpu是arm64-v8a架构的手机，它运行app时，进入jnilibs去读取库文件时，先看有没有arm64-v8a文件夹，如果没有该文件夹，去找armeabi-v7a文件夹，如果没有，再去找armeabi文件夹，如果连这个文件夹也没有，就抛出异常；
如果有arm64-v8a文件夹，那么就去找特定名称的.so文件，注意：如果没有找到想要的.so文件，不会再往下（armeabi-v7a文件夹）找了，而是直接抛出异常。


程序对当前手机cpu架构(比如 armeabi-v7a)做了适配，那手机跑程序时候就直接在这个目录下找对应的so库，如果找不到就直接报错

如果只对armeabi的手机cpu做适配，那么支持armeabi的手机都会去armeabi目录下找对应的so库



 
