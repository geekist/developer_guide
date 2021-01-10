
# Android 打包APK和安装APK的流程


## Android打包apk文件

* step1：通过AAPT工具进行资源文件（包括AndroidManifest.xml、布局文件、各种xml资源等）的打包，生成R.java文件。

* step2：通过AIDL工具处理AIDL文件，生成相应的Java文件。[AIDLAndroid接口定义语言](https://blog.csdn.net/luoyanglizi/article/details/51980630)

* step3：通过Javac工具编译项目源码，生成Class文件。

* step4：通过DX工具将所有的Class文件转换成DEX文件，该过程主要完成Java字节码转换成Dalvik字节码，压缩常量池以及清除冗余信息等工作。**[Android Dex文件及其在热修复中的作用](https://tech.youzan.com/qian-tan-android-dexwen-jian/)**

* step5：通过ApkBuilder工具将资源文件、DEX文件打包生成APK文件。

* step6：利用KeyStore对生成的APK文件进行签名。

* step7：如果是正式版的APK，还会利用ZipAlign工具进行对齐处理，对齐的过程就是将APK文件中所有的资源文件举例文件的起始距离都偏移4字节的整数倍，这样通过内存映射访问APK文件
的速度会更快。

![](https://img-blog.csdn.net/20180417145413634?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUX3d1ZGFv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
