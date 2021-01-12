
# AR

## 1、编译和运行环境

* 创建虚拟设备
adv manager--->create virtual Device...选择pixel2

-->next--->Q---29--x86-Android 10.0

* 确保模拟器已配置为使用最新版本的 OpenGL ES：

在正在运行的模拟器中，点击工具栏中的更多。...
选择 Settings > Advanced > OpenGL ES API level > Renderer maximum (up to OpenGL ES 3.1)。
重新启动模拟器。 系统提示时，请不要保存当前状态。
然后在terminal模式下可以查看：

```java
adb “logcat | grep eglMakeCurrent”
```
如果提示：

… …  …  … D EGL_emulation: eglMakeCurrent: 0xebe63540: ver 3 0 (tinfo 0xd104cb40)
则说明opengl配置好了

* 在虚拟设备上安装ARCore

下载[Google_Play_Services_for_AR_1.22.0_x86_for_emulator.apk](https://github.com/google-ar/arcore-android-sdk/releases)
拖动到模拟器界面中，提示安装，继续即可。

* 下载和运行

下载[souce-code.zip](https://github.com/google-ar/arcore-android-sdk/releases)，编译和运行即可。







