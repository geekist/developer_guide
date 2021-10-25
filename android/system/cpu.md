

## android系统的cpu架构种类


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
 
