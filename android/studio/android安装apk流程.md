
# Android的APK文件安装

## Android的APK文件安装过程

* step1：复制APK到/data/app目录下，解压并扫描安装包。

* step2：资源管理器解析APK里的资源文件。

* step3：解析AndroidManifest文件，并在/data/data/目录下创建对应的应用数据目录。

* step4：然后对dex文件进行优化，并保存在dalvik-cache目录下。

* step5：将AndroidManifest文件解析出的四大组件信息注册到PackageManagerService中。

* step6：安装完成后，发送广播。

![](https://img-blog.csdn.net/20180417145526656?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUX3d1ZGFv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)