

## 蓝牙定义

蓝牙（Bluetooth），一种无线通讯技术标准，用来让固定与移动设备，在短距离间交换数据，以形成个人局域网（PAN）。其使用短波特高頻（UHF）无线电波，经由2.4至2.485 GHz的ISM频段来进行通信。

## 蓝牙发展

![](https://img-blog.csdnimg.cn/20190308165019707.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21lZGlhdGVj,size_16,color_FFFFFF,t_70)

* 蓝牙2.1

使用最广，之前的很多产品都是这个版本，俗称经典蓝牙。

* 蓝牙3.0

又名为高速蓝牙，在2.1的基础上大大提升了传输速度(24Mbps)。

* 蓝牙4.0

最重要的特性是支持省电，即引入了低功耗蓝牙。

* 蓝牙4.1

其目的是为了让 Bluetooth Smart 技术最终成为物联网(Internet of Things)发展的核心动力。

* 蓝牙5.0

在2016年6月发布。在有效传输距离上将是4.2LE版本的4倍（理论上可达300米），传输速度将是4.2LE版本的2倍（速度上限为24Mbps）。蓝牙5.0还支持室内定位导航功能（结合WiFi可以实现精度小于1米的室内定位），允许无需配对接受信标的数据（比如广告、Beacon、位置信息等，传输率提高了8倍），针对物联网进行了很多底层优化。


## ble和传统蓝牙的区别

传统蓝牙：

蓝牙1.0 2.0 3.0 4.0，不过现在已经不再用版本号区分蓝牙了，蓝牙1.0~3.0都是经典蓝牙，在塞班系统就已经开始使用了，确实很经典。

搜索、连接速度慢；传输速度快，数据量大。能耗大

传统蓝牙应用于语音、音乐等对数据量传输比较大的场景。

BLe（Bluetooth Low Energy）

主要特征是能耗低。是搜索、连接的速度更快，传输的速度慢，传输的数据量也很小，每次只有20个字节。

BLE多应用于对实时性要求较高，但对数据传输速率要求比较低的场景，比如血压计、键鼠等设备。

##  GATT介绍

GATT Profile架构图如下：



![](https://img-blog.csdnimg.cn/2019030819465780.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21lZGlhdGVj,size_16,color_FFFFFF,t_70)

 
* GATT Profile可由多个Service组成


* 每个Service由多个Characteristic组成


* 每个Characteristic由属性（Properties）、value 和 0至多个对此Characteristic的描述（Descriptor）所组成。在BLE设备连接建立成功之后读写，就是对Characteristic的读写。


![](https://upload-images.jianshu.io/upload_images/2477378-b618cb71062468b0.png)

